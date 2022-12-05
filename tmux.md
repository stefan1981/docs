# tmux?

A handy tool for dealing with different bash sessions is tmux. It's comparable with gnu screen but by far more advanced. You can get it with:
sudo apt-get install tmux
Launch it by typing:
tmux

## Shortcuts an Commands

Dealing with sessions brings great power. You can set up your multi-windows panes on a remote machine, in a seperate session. Now you can detach from this session and log out. When you later ssh back into this machine you can reattach this session and all your views are restored. Executing commands tmux specific is usually prefixed with the key combination ctrl+b. See the following list what you can do with tmux.

### Session and general
```
tmux ls                             list all running sessions
tmux new -s sessionname             create a new session
tmux attach -t sessionname          open a specific session
tmux a                              attach to last session
tmux kill-session -t sessionname    kill a specific session
ctrl+b s                            list sessions (inside tmux)
ctrl+b :new                         create new session inside tmux
ctrl+b $                            rename current session
ctrl+b d                            detach you from the session
ctrl+b ?                            list all shortcuts
```

### Windows
```
ctrl+b c              create a new (c=create)
ctrl+b &              kill windowbs
ctrl+b ,              rename the current
ctrl+b p              go to previous (p=previous)
ctrl+b n              go to next (n=next)
ctrl+b w              show list of all windows (w=windows)
```

### Panes
```
ctrl+b %              split window into vertical panes
ctrl+b "              split window into horizontal panes
ctrl+b o              switch between panes
ctrl+b x              kill current pane
ctrl+b !              close all panes excepting current
ctrl+b space          change pane layout
ctrl+b {              move current pane to the left
ctrl+b }              move current pane to the right
```

### Restoring tmux after a system reboot

Install the following: TPM (tmux plugin manager) from here
After that write inside: ~/.tmux.conf this lines:
```
set -g @plugin 'tmux-plugins/tmux-resurrect'
set -g @plugin 'tmux-plugins/tmux-continuum'
set -g @continuum-restore 'on'
```
Now you can save/restore the tmus session inside tmus like this:
```
ctrl-b ctrl-s                save session
ctrl-b ctrl-r                restore session
```
