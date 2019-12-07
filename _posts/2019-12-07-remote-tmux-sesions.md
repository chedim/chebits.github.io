---
title: Automated remote sessions in tmux
---

Hello, Stranger!

It's me again, aka chedim. This time I want to share with you my new tmux workflow setup. How can it be interesting to you? Well, consider the following features it supports:

- A ssh-agent session that is shared across all tmux sessions (and, optionally, forwarded to remote hosts) means that you can open new ssh/mosh connections inside tmux and enter your key passphrase only once!
- Remote sessions that let you have multiple tmux windows that, upon opening a new pane, automatically open ssh-shell to assigned to the session remote machine, allowing you to work on that machine in local tmux (almost) in the same way as you do with your local shell!

And here is the full description of my new tmux workflow:

## 1. Configuring login shell
Add the following to your `.bashrc`:

```
export PATH=$PATH:$HOME/bin/

if command -v tmux &> /dev/null; then
  if [ -n "$PS1" ] && [[ ! "$TERM" =~ screen ]] && [[ ! "$TERM" =~ tmux ]] && [ -z "$TMUX" ]; then
    exec ssh-agent bin/start_shell
  elif [ ! -z "$TMUX" ] && [ ! -z "$TMUXSSH" ]; then
    exec bin/remote_shell
  fi
fi
```

## Start_shell script
Put this code into `bin/start_shell`:
```
#! /bin/bash

ssh-add ~/.ssh/id_rsa
tmux a || tmux

```

This script first adds your private key to the ssh-agen session (you will be prompted for its passphrase here), and then either re-attaches to existing tmux session, or creates a new one.

## Remote_shell script
Put this code into `bin/remote_shell`:
```
#! /bin/bash
mosh "$TMUXSSH" || ssh "$TMUXSSH"
```

## rmux script
Put the following code into `bin/rmux`:
```
#! /bin/bash

host=$1
tmux new-session -s $host -d \; \
  -t $host TMUXSSH $host \; \
  send-keys -t $host "export TMUXSSH="$host C-m "remote_shell && exit" C-m
  
switchc -t $host
```

This script starts a new tmux session and binds it to the provided as parameter remote host by creating a session-global variable $TMUXSSH with the name or ip address of the host, then opens an ssh connection to the host and makes target tmux session active. If the session already exists then it just switches to it.

## Final touches
Add these settings to your `~/.tmux.conf`:

```
set-environment -g 'SSH_AUTH_SOCK' ~/.ssh/ssh_auth_sock
set-option -g detach-on-destroy off
```

The first line shares ssh-agent across multiple sessions; the second line instructs tmux to automatically switch to another session upon closing last session window (the default behaviour is to exit tmux).

