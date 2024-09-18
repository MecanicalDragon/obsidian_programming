`echo` - print something
`echo -n` - print something without new line in the end
`base64` - encode in base64
`base64 -w 0` - encode in base64, no line-breaks (other number - X symbols per line)
`grep` - find lines that contain input (`grep "hello" file.txt`)
`grep [options] pattern [file...]` - find lines by regex
`grep -r` - find recursively (`grep -r "error" /var/log`)
`|` - pipelining. Apply output of one command as input of another: `ps aux | grep 8080`

`ps aux` - show running processes
`gsettings list-keys org.gnome.desktop.wm.keybindings` - list shortcuts (*switch-to-workspace-left* is the first to remap)
`nohup`- [[misc#^nohup|run application in background]]
`chmod` - [[CHMOD|access rights]]
`source <file>` - execute commands from a file in the current shell session ^source

`kill -9 <PID>` - kill the process immediately
`kill -15 <PID>` - shutdown the process
`xkill` - point to a window and kill this window's process
