# GitLab CI/CD
## Configuration
Whole CI/CD configuration is written in **YAML format**.
The filename has to be `.gitlab-ci.yml`

## Jobs
- Each step is described by a job.
- A job must contain at least the `script` clause. 
- `script` specify the commands to execute
- `before-script`specify the commands that should run before the `script` command.
```yaml
run_test:
  before_script: 
    - apt-get update && apt-get install make
  script: 
    - make test
```

## Executor
If docker is used by the runner an image can be manually set: 
```yaml
run_test:
  image: python:3.9-slim-buster
```

## Variables
Variables can be added under Settings > CI/CD > Variables
- Masked variables: Variable will not be visible in job logs. 
- Variables can be accessed via $VARIABLE
```yaml
variables:             # pipeline level (global)
  IMAGE_NAME: name    

job_title:            
  variables:           # job level (local)
    IMAGE_NAME: name
    IMAGE_TAG: 1.0
```

## Artifacts
Allow to use the output (for example a directory or file) of one job for another job: 
```yaml
...
  artifacts:
    paths: 
      - build
...
```
After a job has run, the artifacts can be inspected via the GitLab GUI. (On the right site of the job log)

## Services
You can specify an additional image by using the `services` keyword. This additional image is used to create another container, which is available to the first container. The two containers have access to one another and can communicate when running the job.
```yaml
  services: - docker:20.10.16-dind
```

## Structure
### Stages
To force jobs to run after each other, they can be organized in stages. Each job in the same stage runs simultaneously. 
There are some predefined stages, such as: test, build, .pre
```yaml
stages:
  - test
  - build

run_tests:
  stage: test
  ...

build_image: 
  stage: build
  ...
```
### Basic rules
- Failing fast: Test which are often failing should be executed first (linter / unit test)

## Rules
Conditions for a job to run
```yaml
  rules:
    - if: $CI_COMMIT_REF_NAME == main     # checks if the current branch is the main branch
    - if: $CI_COMMIT_REF_NAME == $CI_DEFAULT_BRANCH   # checks if the current branch is the default branch
```
## Continuous Delivery
The Job for the main branch has to be initiated manually.
```yaml
  when: manual
```
## Environments 
Define different environments like stage and production
- Variables can be assigned to specific environments 
- Predefined variables are available for environments 
- Assign a job to an environment: 
  ```yaml
  environment: staging
  ```
## Templates
Templates can be configured with a `.` in front of the name: 
```yaml
.deploy
```
A template can be used by other jobs: 
```yaml
  extends: .deploy
```

## Build
### Docker
```yaml
build_image: 
  image: docker:20.10.16
  services:
    - docker:20.10.16-dind
  variables: 
    DOCKER_TLS_CERTDIR: "/certs"
  before_script: 
    - docekr login -u $REGISTRY_USER -p $REGISTRY_PASS registry
  script: 
    - docker build -t registry/demo-app:python-app-1.0 . 
    - docker push registry/demo-app:python-app-1.0
```

## Deployment
### Remote Access
- To establish a remote connection to a server via SSH add the private SSH-Key to the Variables section in GitLab. 
	- Type has to be File. 
	- A blank line has to be added at the end of the file 
```yaml
deploy:
  before_script: 
    - chmod 400 $SSH_KEY
  script: 
    - ssh -o StrictHostKeyChecking=no -i $SSH_KEY root@1.2.3.4 "
	    docekr login -u $REGISTRY_USER -p $REGISTRY_PASS registry &&
	    docker ps -aq | xargs docker stop | xargs docekr rm &&      # stop and remove any container
        docker run -d -p 5000:5000 registry/image:tag"
```

### AWS
```yaml
deploy to s3:
  stage: deploy
  image: 
    name: amazon/aws-cli:2.4.11
    entrypoint: [""]
  script: 
    - aws --version
    - echo "Hello S3" > test.txt
    - aws s3 cp test.txt s3://$AWS_S3_BUCKET/test.txt    
```

## Debugging
- To temporary disable a job you can add a . in front of it
  ```yaml
  .job:
     ...
  ```
