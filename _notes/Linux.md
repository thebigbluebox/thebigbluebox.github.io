---
title:  "Linux Basics"
date:   2016-5-3 9:00:00
description: Back to the basics
categories:
 - category: "linux"
---

# Linux Basics
Things I learned from just reading linux essentials

* Pwd print working directory shows current path

#### Ls list what is currently in our directory
1. Ls [option] [directory]
2. -l flag is long listing
3. drwxr-xr-x  12 TaiRui  staff   408  4 Apr 19:46 Applications
* D <-directory type if nothing there it is not a directory
* RWXR permissions
* 12 -> number of blocks 
* TaiRui owner of the directory / file
* Group that the owner is part of
* File size in MiB
* The time it was last changed
* Folder name / File name
# Paths
* Absolute which are absolute locations based from your home level ~
  * Relative which can be achieved from current / or from above ../, or from your user home level ~
  * And change directories using cd [directory]
* Important folders in your NIX system
  * /etc -> stores configuration files for the system
  * /var/log -> stores logs for various system components
  * /bin -> location of several commonly used programs
  * /usr/bin -> Another location for programs [http://unix.stackexchange.com/questions/5915/difference-between-bin-and-usr-bin]
* Everything is a file in UNIX
  * Everything in a NIX system is a file, this was set as a core principal design
  * File extensions don't matter to the system, as it relies on the file header within the file to tell the nix system what type of file it is
		* We can check a file's type using the file command
  * Files are case sensitive and even commands are case sensitive as well
  * Spaces!
		* How do we deal with spaces in the file or directory? Either by using quotes cd 'Family Photos'
		* Or via an escape character cd Family\ Photos the escape was the \ <- there is a space after that escape character
  * Hidden files in nix systems all begin with a full stop period, so in your home directory configurations can be seen by doing a ls -a
* Man Pages
  * Man [command] which will show you a list of all configuration with the command shows you how to use it
  * Searching can be done with man -k <keyword> or while in a man page /<keyword> then press n to browse next and next
* File and Directory Manipulation
  * Mkdir [options] <directory>
		* The creation of directory
		* -p will create the parent directory if you where to create a directory /parent1/folder
		* -v will log the creation of the folders 
		* -pv combination p and v, lol so we list out the creation of multiple parent folders and such
  * Rmdir removes the directory =
  * Touch [option] <file> we don't just create a file we TOUCH ONE 
  * Cp copying a file or folder
		* Cp source_file new_file
		* Cp -r source_folder new_folder
  * MV moving a file or folder
		* Mv source file new_source_location
		* Mv -r source_folder new_source_location_folder
		* To rename we can just do the same for moving folders and files just change the name
  * Rm removes a file
		* Rm [option] <file>
		* Rm -r <- removes recursively
* Command line Text Editors
  * Vi - Vim the thing that everyone is scared of
   * Open a file vi [options] <file.txt>
   * Read only view <file.txt>
   * Text commands
    * i will change the opened mode to insertion mode which allows for the modification of the file
    * ZZ save and exit
    * :q! quit don't save
    * :w save
    * :wq save and quit
    * Arrow keys - move the cursor around
    * j, k, h, l - move the cursor down, up, left and right (similar to the arrow keys)
    * ^ (caret) - move cursor to beginning of current line
    * $ - move cursor to end of the current line
    * nG - move to the nth line (eg 5G moves to 5th line)
    * G - move to the last line
    * w - move to the beginning of the next word
    * nw - move forward n word (eg 2w moves two words forwards)
    * b - move to the beginning of the previous word
    * nb - move back n word
    * { - move backward one paragraph
    * } - move forward one paragraph
    * x - delete a single character
    * nx - delete n characters (eg 5x deletes five characters)
    * dd - delete the current line
    * dn - d followed by a movement command. Delete to where the movement command would have taken you. (eg d5w means delete 5 words)
    * u - Undo the last action (you may keep pressing u to keep undoing)
    * U (Note: capital) - Undo all changes to the current line
  * Cat (concatenate) -> used to view files and logs right in the cmdline
  * Less -> used to view large files with ability to scroll 
* Wildcards
  * *  All seeing star, matches n characters of whatever
  * ? Matches one unknown character ?A* matches the second letter with A
  * [] range operator, matches any letter within the specified range ?[sv]* matches second letter with either s or v or [0-9] matches numbers from 0-9
* Permissions
  * 3 things for a file
		* Read [r]
		* Write [w]
		* Execute [x]
  * 3 different types of permissions
		* Owner single person owns the file, permission could be granted to other users to access the file
		* Group
		* Others 
  * Chmod to change the permission to the file
		* 3 components : 
    * Who are we changing the permission for user, owner or all? 
    * Are we granting or revoking permissions
    * Which permission are we setting (R W X)
		* And we can do short hand such as 777 which in octal speak will set the individual bit values for the permissions at the [owner,group,others] group, and within that the bit representation of the value is used to determine the permission level
* Filters
  * Filter is a program that  accepts textual data and transforms it in a particular way
  * Head prints out the head of the document up to n number of lines
  * Tail prints out the bottom of the document up to n number of lines
  * Sort sorts the printout of the document by default alphabetically
  * Nl shows line numbers for the print out
  * Wc word count, character count.. Etc look in man pages
  * Cut useful for column data, csv… tabbed data, but you can cut out a certain column using this command
  * Sed stream editor, can edit an input stream's data, or just do search and replace within a document
  * Uniq gives output of only unique data, really useful for column data again
  * Tac <-reverse of cat, going from the bottom to the top
* Regex
  * . (dot) - a single character.
  * ? - the preceding character matches 0 or 1 times only.
  * * - the preceding character matches 0 or more times.
  * + - the preceding character matches 1 or more times.
  * {n} - the preceding character matches exactly n times.
  * {n,m} - the preceding character matches at least n times and not more than m times.
  * [agd] - the character is one of those included within the square brackets.
  * [^agd] - the character is not one of those included within the square brackets.
  * [c-f] - the dash within the square brackets operates as a range. In this case it means either the letters c, d, e or f.
  * () - allows us to group several characters to behave as one.
  * | (pipe symbol) - the logical OR operation.
  * ^ - matches the beginning of the line.
  * $ - matches the end of the line.
* Piping!! And Redirection
  * Every program that we run in the commandline has 3 different streams that can be connected to it
  * STDIN standard input data fed into the program
  * STDOUT standard output data outputted from the program
  * STDERR which are the error output stream
  * Redirecting to a file
		* If we wish to take the output stream of a application and save it to a file we will use the '>' greater than symbol to a file that you want as the output
		* If we redirect to afile that’s name isn't already created then a new file will be created for you, however if redirecting to an existing file that file will be over written 
    * But we can get the data appended to the file using the >> operator
  * Redirecting from a file
		* Using the '<' less than operator we can actually redirect a file to be as an input to a program, we redirect into the stdin stream
  * Redirecting to standard error
		* Using the 2> operator where we identify the stream by its number stream 2 being the err stream, we can direct an error the error stream which will be printed out on the commandline
		* ls -l video.mpg blah.foo > myoutput 2>&1 will redirect the output stream of err into the output file stream to be saved into the file myoutput
  * Piping
		* We can pipe the output of one file into another file using the | operator, it will pipe the output from the application on the left into the one on the right
* Process management
  * Top <-shows processes in ram gives a live output
  * PS aux to show all processes running
  * Killing a process can be done with kill <pid> or we can truly kill by kill -9 <pid> which is a force kill which will not wait and kill it
  * Running processes in the background
		* You can run processes in the background by adding the & operator at the end of a commandline to move the process in the background
		* To move a job or process back into the foreground use the 'fg' command with the job id to bring that process into the foreground
		* Ctrl z will bring the currently running foreground application into the background
* Bash Scripting
  * http://ryanstutorials.net/linuxtutorial/scripting.php
		
		
				
		
