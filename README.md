# memcheck 🐘

> *"Why is my fan screaming?"* — you, 40 seconds before running this.

A tiny bash script that points at the processes eating your RAM and, if you
ask nicely, takes them out back. No dependencies, no config, no Electron app
using 900 MB to tell you that Chrome is using 900 MB.

## Use

```sh
memcheck             # everything using >= 1.0% RAM, biggest glutton first
memcheck 0.5         # lower the bar: >= 0.5% RAM
memcheck kill <PID>  # politely SIGTERM, then SIGKILL if it pretends not to hear
```

Sample output (names changed to protect the guilty):

```
Processes using >= 1.0% RAM:

PID        %MEM   %CPU    RSS(MB)  COMMAND
4242        9.8    3.1     1604.2  Google Chrome Helper (Renderer)
1337        6.2   12.0     1011.7  Slack Helper
666         4.4    0.0      719.3  that-one-docker-container

To kill a process: memcheck kill <PID>
```

`kill` always shows you the process and asks `[y/N]` first. It will not murder
anything on your behalf without consent. It's a script, not a sociopath.

## Install

```sh
chmod +x memcheck
ln -sf "$PWD/memcheck" /opt/homebrew/bin/memcheck
```

## How it works

`ps aux` → `awk` filter on `%MEM` → `sort` → another `awk` to make it pretty.
That's the whole trick. Commands are trimmed to the binary name so rows stay
on one line even when a process has a 400-character argument list (looking at
you, every Node app ever).

## ☕ Support

Free, and uses 0 MB of RAM while sitting on disk. If it caught a glutton for you, you can buy me a coffee:

[![Support me on Ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/gigacook)
