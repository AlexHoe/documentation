# Command Reference

## which
- shows the path of the command
- only searches in directories listet in the PATH variable
```sh
which emacs
```

## whereis
- show the path of the command and its man pages
- searches in all directories, config files and man pages
```bash
whereis fstab
```

## locate
- searches in the complete path and filename 
- uses a database for the search (fast)
```bash
locate <muster>
updatedb (update the database)
```

## dd

USB Stick erstellen

```bash
dd if=ubuntu.img of=/dev/sdc bs=4M status=progress
```

## tail

Opens the file at the end, and watches for file changes

```bash
tail -f /var/log/system.log
```

You can print the last 10 lines in a file:

```bash
tail -n 10 <filename>
```

You can print the whole file content starting from a specific line using + before the line number:

```bash
tail -n +10 <filename>
```

## cat

You can print the content of multiple files:

```bash
cat file1 file2
```

Using the output redirection operator > you can concatenate the content of multiple files into a new file:

```bash
cat file1 file2 > file3
```

Using >> you can append the content of multiple files into a new file, creating it if it does not exist:

```bash
cat file1 file2 >> file3
```

When you're looking at source code files it's helpful to see the line numbers. You can have cat print them using the -n option:

```bash
cat -n file1
```

## alias

```bash
alias ll='ls -al'   # create alias
alias               # list all alias
unalies ll          # remove alias
```

The alias will work until the terminal session is closed.

-   To make it permanent, you need to add it to the shell configuration. This could be ~/.bashrc or ~/.profile or ~/.bash_profile if you use the Bash shell, depending on the use case.

## tar

The tar command is used to create an archive, grouping multiple files in a single file.

This command creates an archive named archive.tar with the content of file1 and file2:

```bash
tar -cf archive.tar file1 file2
```

The c option stands for create. The f option is used to write to file the archive.

To extract files from an archive in the current folder, use:

```bash
tar -xf archive.tar
```

the x option stands for extract.

And to extract them to a specific directory, use:

```bash
tar -xf archive.tar -C directory
```

You can also just list the files contained in an archive:

```bash
tar -tf archive.tar
```

tar is often used to create a compressed archive, gzipping the archive.

```bash
tar -czf archive.tar.gz file1 file2
```

Unpack and unzip

```bash
tar -xf archive.tar.gz
```

## gzip

compress a file

```bash
gzip filename
```

This will compress the file, and append a .gz extension to it. The original file is deleted.

-   To prevent this, you can use the -c option and use output redirection to write the output to the filename.gz file:

```bash
gzip -c filename > filename.gz
gzip -k filename
```

Levels range from 1 (fastest, worst compression) to 9 (slowest, better compression), and the default is 6.

```bash
gzip -1 filename
```

You can compress all the files in a directory, recursively, using the -r option:

```bash
gzip -r a_folder
```

gzip can also be used to decompress a file, using the -d option:

```bash
gzip -d filename.gz
```

## gunzip

```bash
gunzip filename.gz
```

You can extract to a different filename using output redirection using the -c option:

```bash
gunzip -c filename.gz > anotherfilename
```

## ln

```bash
ln <original> <link>
```

```bash
# Soft link
ln -s <origin> <link>
```

## find

When no arguments are given `find` lists all files in the current directory and all of its subdirectories

Find all the files under the current tree that have the .js extension and print the relative path of each file that matches

```bash
find . -name '*.js'
```

Find directories under the current tree matching the name "src":

```bash
find . -type d -name src
```

-   Use -type f to search only files, or -type l to only search symbolic links.
-   name is case sensitive. use -iname to perform a case-insensitive search.
-   You can search under multiple root trees:

```bash
find folder1 folder2 -name filename.txt
```

Find directories under the current tree matching the name "node_modules" or 'public':

```bash
find . -type d -name node_modules -or -name public
```

You can also exclude a path using -not -path:

```bash
find . -type d -name '*.md' -not -path 'node_modules/*'
```

Search for files which belongs to a specific group:
```bash
find /home -group users
```

You can search files that have more than 100 characters (bytes) in them:

```bash
find . -type f -size +100c
```

Search files bigger than 100KB but smaller than 1MB:

```bash
find . -type f -size +100k -size -1M
```

Time:

-   `ctime`: changed ownership, permission
-   `atime`: last accessed
-   `mtime`: last modified

Search files edited more than 3 days ago:

```bash
find . -type f -mtime +3
```

You can delete all the files matching a search by adding the -delete option. This deletes all the files edited in the last 24 hours:

```bash
find . -type f -mtime -1 -delete
```

You can execute a command on each result of the search. In this example we run cat to print the file content:

```bash
find . -type f -exec cat {} \;
```

`ok` behaves the same as `exec` but aks for permission for each task

## history

repeat last command

```bash
!!
sudo !!
```

use parameter of the last command /co

```bash
!$
```

use all parameters

```bash
!*
```

use only first parameter

```bash
!1
```

change format of history via environment variable: HISTTIMEFORMAT

e.g. Date (F) and Time (T):

```bash
HISTTIMEFORMAT="%F %T: "
```

run history command via number

```bash
!24
```

save timeline in file

```bash
history -w myhistory
```

read timeline from file

```bash
history -r myhistory
```

delete history

```bash
histroy -c
```

disable history

```bash
set +o history
```

enable history

```bash
set -o history
```

## man

manual for most of the commands

```bash
man ls
```

tldr: short man pages with examples

```bash
tldr ls
```

## grep
Search for a specific phrase within a file

```bash
grep document.getElementById index.md
```

Using the -n option it will show the line numbers:

```bash
grep -n document.getElementById index.md
```

One very useful thing is to tell grep to print 2 lines before and 2 lines after the matched line to give you more context. That's done using the -C option, which accepts a number of lines:

```bash
grep -C 2 document.getElementById index.md
```

Search is case sensitive by default. Use the -i flag to make it insensitive.
```bash
grep -i <phrase> <file>
```
Shows all lines which not include the search phrase
```bash
grep -v <phrase> <file>
```
Remove all comments from a file
```bash
grep -v '^#' configfile
```

## sort

Sort a file by name

```bash
sort test.txt
```

Use the r option to reverse the order:

```bash
sort -r test.txt
```

Sorting by default is case sensitive, and alphabetic. Use the --ignore-case option to sort case insensitive, and the -n option to sort using a numeric order.

You can use the -u option to remove duplicate lines:

```bash
sort -u test.txt
```

## uniq

uniq will only detect adjacent duplicate lines. This implies that you will most likely use it along with sort:

```bash
sort test.txt | uniq
```

You can tell it to only display duplicate lines, for example, with the -d option:

```bash
sort dogs.txt | uniq -d
```

You can count the occurrences of each line with the -c option

Use the special combination to then sort those lines by most frequent:

```bash
sort dogs.txt | uniq -c | sort -nr
```

## diff

diff will process two files and will tell you what's the difference.

```bash
diff dogs.txt moredogs.txt
```

Using the -y or -u option will compare the 2 files line by line:

```bash
diff -y dogs.txt moredogs.txt
diff -u dogs.txt moredogs.txt
```

Comparing directories works in the same way. You must use the -r option to compare recursively:

```bash
diff -ur dir1 dir2
```

In case you're interested in which files differ, rather than the content, use the r and q options

```bash
diff -rq dir1 dir2
```

## chown

The owner (and the root user) can change the owner to another user, too, using the chown command:

```bash
chown <owner> <file>
```

It's rather common to need to change the ownership of a directory, and recursively all the files contained, plus all the subdirectories and the files contained in them, too:

```bash
chown -R <owner> <file>
```

Through this command you can change the group simultaneously while you change the owner:

```bash
chown <owner>:<group> <file>
```
## chgrp
You can also just change the group of a file using the chgrp command:

```bash
chgrp <group> <filename>
```

## chmod

You type `chmod` followed by a space, and a letter:

-   `a` stands for _all_
-   `u` stands for _user_
-   `g` stands for _group_
-   `o` stands for _others_

Then you type either + or - to add a permission, or to remove it. Then you enter one or more permission symbols (r, w, x):

```bash
chmod a+r filename #everyone can now read
chmod a+rw filename #everyone can now read and write
chmod o-rwx filename #others (not the owner, not in the same group of the file) cannot read, write or execute the file
chmod u+s filenmae # set setuid bit
chmod g+s filename # set setgid bit
chmod +t filename # set sticky bit
```

-   `0` no permissions
-   `1` can execute
-   `2` can write
-   `3` can write, execute
-   `4` can read
-   `5` can read, execute
-   `6` can read, write
-   `7` can read, write and execute

```bash
chmod 777 filename
chmod 755 filename
chmod 644 filename
```
## usermod
Add user to a group
```bash
usermod -aG <group> <user>
```
## umask
```bash
umask      # show current values
umask 27   # set new values
```
## getfacl
```bash
getfacl <datei>     # shows permissions in ACL form
```
## setfacl
```bash
setfacl -m alex:rw <datei>         # add read + write permission for user alex
setfacl -m g:docuteam:rw <datei>   # add read + write permission for group docuteam
setfacl -m theri:- <datei>         # remove all permissions for user theri
```
## shred
Festplatte 10x überschreiben und anschließend mit Nullen füllen:
```bash
shred -vzf -n 10 /dev/sda
```
-   `-n`, --iterations=N  
    Instead of the default (3) times, overwrite the data N times.
-   `-z`, --zero  
    Add a final overwrite with zeros to hide shredding.
-   `-f`, --force  
    Force the permissions to allow writing if necessary.
-   `-v`, --verbose  
    Show progress in detail.
-   `-u`, --remove  
    Truncate and remove file after overwriting.

## SMART
Installation 
```bash
apt install smartmontools
```
Test starten 
```bash
smartctl -t <short|long|conveyance|select> /dev/sdc
```
Anzeigen der Testergebnisse
```bash
smartctl -a /dev/sdc     # Alles ausgeben
smartctl -l /dev/sdc     # Nur Testergebnisse
```

## wget

## curl