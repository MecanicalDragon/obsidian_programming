## Navigation
`./` - current directory
`../` - parent directory
`/path` - path from root
`~/<path>` - path from user home
`cd <path>` - go to the directory with relative or static path
`pwd` - print current directory path
`ls` - show files and dirs in current directory
`ls -l` - show files and dirs in current directory with info
`ls -lh <file>` - show file with info and human-readable file size
`ls -a` - show all files including invisible files
## Files
`mkdir` - create directory
`touch` - create file
`cat` - print file content
`cp` - copy file
`mv` - move file
`rm -f` - remove file
`rmdir` - remove directory
`which <file>` - location of executable `<file>`
`vim` - modify file content with Vim

`find <path> -name <pattern>` - search files in *path* containing *pattern* in the name
	`-iname` - case-insensitive
	`-type f`, `-type d` - search only regular files or directories
	`-size +n`, `-size -n` - larger or smaller than *n* characters
	`-print` - print paths of matched files
	`-empty` - empty files and directories
	`-delete` - delete files that match
	`-exec <CMD> {} \;` - execute command on each file. `{} \;` - mandatory part that conveys the file itself to a command as an argument.
	`execdir <CMD> {} \;` - execute command on each file from a relative file directory. `{} \;` - mandatory part that conveys the file itself to a command as an argument.
`ln -s <file> <link>` - create a link for a file
`split -l`, `split -b` - split file to parts by lines or bytes
`tar -cvf <TAR> <FILE>` - add *FILE* to *TAR* archive
`tar -xvf <TAR> -C <DIR>` - extract *TAR* archive to the directory *DIR*
## Utilities
`sh` - execute shell script`echo` - print something
`echo -n` - print something without new line in the end
`echo "hello" > file` - replace file content with "hello"
`echo "hello" >> file` - add "hello" to the end of the file content
`env` - print environment
`base64` - encode in base64
`base64 -w 0` - encode in base64, no line-breaks (other number *X* - X symbols per line)
`grep` - find lines that contain input (`grep "hello" file.txt`)
`grep [options] pattern [file...]` - find lines by regex
`grep -i` - grep with case ignoring
`grep -v` - grep non-matching lines
`grep -r` - find recursively (`grep -r "error" /var/log`)
`tr <SET1> <SET2>` - replace characters from *SET1* to characters from *SET2*
`tr -d <SET>` - delete characters from the *SET*
`|` - pipelining. Apply output of one command as input of another: `ps aux | grep 8080`
## Network
`ss -a` - show network connections (hosts+ports)
## Syntax
`${}` - variable usage: `echo "Hello, ${name}!"`
`${#var}` - length of the string
`${var:-default}` - var or default value if empty
`${var:offset:length}` - substring
`$()` - result of the command execution: `file_count=$(ls | wc -l)`
`cmd1 && cmd2` - run cmd2 if cmd1 succeeds
`cmd1 || cmd2` - run cmd2 if cmd1 fails

## Service
`man <CMD>` - command manual. (`shift+g` – to the end, `q` - quit)
`ps aux` - show running processes
`gsettings list-keys org.gnome.desktop.wm.keybindings` - list shortcuts (*switch-to-workspace-left* is the first to remap)
`nohup`- [[OS/Linux/misc#^nohup|run application in background]]
`chmod` - [[CHMOD|access rights]]
`systemctl` - manage system files and daemons (`start`, `stop`, `enable` (make active), `daemon-reload` (reload configuration), `restart`, `status`)

`su` - switch user to *root* starting a new shell session. Requires the target user's password
`su <USER>` - switch user to *USER* starting a new shell session. Requires the target user's password
`source <file>` - execute commands from a file in the current shell session ^source
`source /etc/environment` - apply env vars to the session
`source ~/.profile`, `source ~/.bashrc` - apply configuration to the session.
`sudo` - execute a command with *root* rights. Requires your own password
`sudo -u <USER>` - execute a command with *USER* rights. [[OS/Linux/misc#^sudousers|sudousers]]. Requires your own password.
`exit` - exit the current shell session

`kill -9 <PID>` - **SIGKILL** - kill the process immediately on a kernel level
`kill -15 <PID>` - **SIGTERM** - shutdown the process; send to the application a command to shut down actively, allowing JVM to apply registered hooks before termination.
`xkill` - point to a window and kill this window's process
## Examples
Concatenate all files in the directory to a string, separate by '||' (', ' is used by default), assign the result to a variable *somevar*:
`somevar="$(ls -xm <path> | tr ', ' '|')"`
Find all files in a directory, execute script for each of them using file itself as a parameter *p*:
`find <path> -type f -exec script.sh -p='{}' \;`
Delete all files and dirs created before some timestamp:
`touch mark.start -d "2022-10-01 00:00"`
`find /tmp -type d ! -newer mark.start -exec rm -rf {} +`
Increase available disk space:
`sudo lvextend -l +30%FREE /dev/rootvg/lvopt`
`sudo xfs_growfs /dev/mapper/rootvg-lvopt`
`sudo lvextend -l +100%FREE /dev/rootvg/lvvar`
`sudo xfs_growfs /dev/mapper/rootvg-lvvar`
