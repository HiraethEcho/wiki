---
dg-publish: true
---

https://github.com/SuperManito/LinuxMirrors

```
bash <(curl -sSL https://linuxmirrors.cn/main.sh)
```

```
bash <(curl -sSL https://linuxmirrors.cn/docker.sh)
```

```
bash <(curl -sSL https://linuxmirrors.cn/docker.sh) --only-registry
```

### file

use following to find file type. `-L` means follow the symbolic link

```
file --mime-type -L file
```

use following shell scripts to set mime (need rofi)

```sh
#!/bin/sh

FILETYPE=$(xdg-mime query filetype $1)
APP=$( find /usr/share -type f -name "*.desktop" -printf "%p\n" | sed 's/\/.*\///g' | rofi -threads 0 -dmenu -i -p "select default app")
echo $APP
xdg-mime default "$APP" "$FILETYPE"
echo "$APP set as default application to open $FILETYPE"
```

### stat

`stat`展示文件状态，例如

```
stat /

  File: /
  Size: 122       	Blocks: 0          IO Block: 4096   directory
Device: 0,28	Inode: 256         Links: 1
Access: (0755/drwxr-xr-x)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2025-05-10 23:46:03.122616066 +0800
Modify: 2025-05-11 21:26:49.474145593 +0800
Change: 2025-05-11 21:26:49.474145593 +0800
 Birth: 2025-01-01 14:58:38.377746860 +0800
```

### type

```
  Display the type of command the shell will execute.
  Note: All examples are not POSIX compliant.
  More information: <https://www.gnu.org/software/bash/manual/bash.html#index-type>.

  Display the type of a command:

      type command

  Display all locations containing the specified executable (works only in Bash/fish/Zsh shells):

      type -a command

  Display the name of the disk file that would be executed (works only in Bash/fish/Zsh shells):

      type -p command

  Display the type of a specific command, alias/keyword/function/builtin/file (works only in Bash/fish shells):

      type -t command
```
