# Part III: Shell Operations

## Objectives

## Deliverables:
Choose one of the following:

1. Record a video of yourself performing the tasks described in part 1
2. Create a file that lists the commands you used to perform the tasks described in part 1
  - Hint: `history | tail -11`

then...

3. Complete part 2. Answer the questions in the doc, then schedule a time to meet with your mentor and discuss the commands and how they work

### Part 1: Creating, adding to, reading and sorting files
  - Use `echo` and text redirection `>` to create a file and add at least one line of text
  - Use `echo` and text redirection `>>` to append lines to a second file
  - Use `cat <file1> <file2>` to output the contents of both files
  - Use cat and text redirection to create a third file
  - In one terminal windows use text redirection and `echo` to append lines to a file, in a second terminal window use `tail -f` to view the lines being appended. Stop the `tail -f`
  - Use `tail -f | grep <pattern>`  and continue to append lines to the file. Notice that only lines that match your pattern are displayed by grep.
  - Do the above, but invert the match so only lines without the pattern are displayed.
    - `grep -v`
  - Use `cat` and `sort` to display the contents of all the files alphabetically.
    - `cat three.txt | sort`
  - Use `cat`  and `wc -l` to count the number of lines in all the files.
    - `cat three.txt | wc -l`
  - Use `head` to display the first 3 items from the above step

### Part 2
  - Inspect your environment with `env`
  - `export` variable on the command line `export TESTVAR1=testvar1`
  - Use `grep` to find that variable in your environment
    - `env | grep $TESTVAR1`
  - Set a second variable on the command line without exporting it
    - Is that variable in the environment?
      - Without `export` the variable is only available to the shell
      - With export, the variable is added to the environment is available to all processes(?)
      - Question: is this related to the `source` command ?
      - https://jvns.ca/blog/2017/03/26/bash-quirks/
  - `export` that variable. No need to re define it.
    - Does it show up in the `env` now?    
  - Use `which` to find the path of the `grep` binary
    - `which grep`
  - What are the permissions on the `grep` binary? `ls -l`
    - `-rw-r--r--` : read and write access
    - http://www.zzee.com/solutions/linux-permissions.shtml
  - Make a directory `bin` in your home directory
  - Create a script that displays ‘Hello World’ and the in the `$HOME/bin` directory.
    - Can you run it without using the `bash` or `sh`  command?
      - No. I have to change the file permission from having just read and write access to having access to execute
  - What are the permissions on the files you created above?
    - read and write access
  - Make the script executable using `chmod +x`
    - `chmod +x say_hello.sh`
    - Now the permissions look like this: `-rwxr-xr-x`
  - Find use `env`  and `grep` to find the `$PATH` environment variable
    - `env | grep $PATH`
  - Add  `$HOME/bin` to your `$PATH`, be sure to `export` it
    - The script should now be executable from anywhere without using its full path
    - Can you find your script using `which`
      - `which say_hello.sh`
  - Open a new terminal window.
    - Can you still use `which` to find your script?
    - Why or why not?
    - Is `$HOME/bin` still in your `$PATH`
  - Find hidden files in your home directory using `ls -a $HOME`
  - Edit `.bash_profile` to make your change to `$PATH` persist
    - The difference between `.bash_profile` and `.bashrc` and why OSX is different: http://www.joshstaiger.org/archives/2005/07/bash_profile_vs.html
      - add `export PATH=$PATH:$HOME/bin` to `.bash_profile`
  - Three ways to set env variables:
    - foo=bar  (local scope)
    - export foo=bar (global scope inside your terminal window => 1 session)
    - putting the declaration into a file and sourcing the file x
  - `echo "echo $var" >>`  into your script
  - Set  `var=test` in your terminal and run the script.
    - Does it output “test”
  - `export var` and run the script.
    - Does it output “test”
    - Why or why not?
  - Open a new window and run the script
    - Does it output “test”
    - Why or why not?
  - `echo "export var2='exported in the script'" >>` into your script and run it
    - Did  `$var2` get set in your current shell?
    - Why or why not?
      - No. I need to reload the script with `source`
      https://linuxize.com/post/bash-source-command/
  - `source` your script and see if `$var2` is set this time.
    - Did  `$var2` get set in your current shell?
      - yes
    - Why or why not?
      - `source` runs the script in the current environment so the scope overlaps. If you do not `source` it runs in a sub-shell and cannot set new variables in the current session.

  - Create another executable script that outputs “Goodbye World” in you home directory but not in your `bin` directory. Create a symlink to the goodbye_world script in the `bin` directory so that it is accessible if your path is set correctly.
  https://www.youtube.com/watch?v=l_1Q3DG3uoE
  - Inspect your symlink using `ls -l`
    - `lrwxr-xr-x`
    - You should now be able to run your new script from anywhere even though it is only a symlink in the `bin` directory
      - When is this pattern helpful?
        - When you have something that is used in multiple projects and you don’t want to recreate the resource in multiple places
  - Use Homebrew to install python, ansible and terraform
  - Use `ls -l $(which <program>)` to see if the binaries are linked. If they are not use `brew link` and `which` to see where the binaries exist
    - ex: `ls -l $(which ansible)`
  - Inspect the path of each binary and determine if it is a symlink and if so, where the actual binary is on the file system
    - `ls /usr/local/bin/terraform` shows me that terraform is a symlink (on my machine, it’s pink and has an `@` at the end of the name)
    - `realpath /usr/local/bin/ansible` => /usr/local/Cellar/ansible/2.5.4/libexec/bin/ansible
    - `realpath /usr/local/bin/terraform` => /usr/local/Cellar/terraform/0.11.7/bin/terraform
    - `realpath /usr/local/bin/python` => /usr/local/Cellar/python@2/2.7.15_1/Frameworks/Python.framework/Versions/2.7/bin/python2.7
  - Symlinks
    - To keep a directory organization and have bin files and keep in organized like brew install
