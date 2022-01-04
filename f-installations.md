# Installations

## Command Line Basics

- "Directory" essentially means location
- Home directory is the default location of a new terminal. It is where settings files and the Desktop folder are stored. `~` is a shortcut for the home directory path.
- `pwd` shows the current directory you are working in.
- `ls` lists files and folders that are in your current directory.
  - `ls pathOfDirectory` does the command for another directory. You can type the path of the other directory either from your pwd or from home (using `~`).
  - This doesn't show hidden files (starting with a `.`). Show hidden files with `ls -a`.
  - Show names AND detailed info about files with `ls -l`.
  - `ls -lh` gives human-readable files sizes instead of numer of bytes.
  - `-S` sorts by size; `-t` sorts by last modified time; `-r` does a reverse sort;
- `ln sourceFile.txt targetFile.txt` creates a hard link: The target file is an identical copy of the source files that gets updated automatically when the source file is modified.
  - You can not create a hard link to a file on a different file system. To do that you need to use symbolic links.
  - If the source file gets deleted, the target file will continue to live as an independent file, despite the severed link.
  - If the target file already exists you will get an error. You can force the link with `ln -f`.
  - Doesn't work for directories.
- `ln -s` creates a symbolic link. The target file is a shortcut that points to the source file.
  - Also works for directories
- `cd newDirectoryPath` to change pwd.
- `cd ..` to change pwd to parent directory.
- `mkdir newDirectoryName` to make a new directory in pwd.
  - `mkdir -p newdir1/newdir2` to make nested directories.
- `cp sourceFile.txt ~/targetPath/targetFile.txt` creates a copy of the source file at a new location with a new name. If you only specify the target file path and not the target file name, it will have the same name as the source file.
  - You can copy multiple source files as long as the last argument is the target directory: `cp source1.txt source2.txt targetdir`.
  - `-R` to copy a directory
- `rm file.txt` to remove files.
  - `-R` to remove directory.
- `mv file.txt newLocationPath` to move files (also works for moving directories).
  - You can optionally specify a new file name after new the new location path.
  - Command can be used for renaming files: `mv file.txt newName.txt`.
- `command1 >> file.txt` the output of the command on the left is appended to the file on the right.
- `touch file.txt` to create a new file.
- To copy/past in terminal, highlight the text and do `Ctrl+Shift+C` or `Ctrl+Shift+V`.
- `code .` to open all files in pwd in VS Code. `code file.txt` to open a specifc file.
- `Ctrl+alt+T` to open terminal on linux.