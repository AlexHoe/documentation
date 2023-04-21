# Command Reference

- Help: `command /?`
- `ipconfig`: shows network adapter
- `attrib`: show and set file parameters
  ```powershell
  attrib file.txt       # show attribute 
  attrib +H file.txt    # hide file
  ```
- delete file: `del file.txt`
- show text file: `type file.txt`
- copy file: `copy file.txt /dir`
- move file: `move file.txt /dir`
- copy output to clipboard: `command | clip`