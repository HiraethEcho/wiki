---
title: sed cheatsheet
toc: true
tags: 
date: 2025-05-01
dg-publish: true
---

# sed

## tldr


## tldr

```sh
sed -i "[range]s/[patern]/[replacement]/[options]" file_*
sed -i "2,5s|[patern]|[replacement]|[options]" file_*
```

### Search for text

Search for a string and only print the lines that were matched

```sh
$ sed -n '/hello/p' file.txt
```

Case insensitive search

```sh
$ sed -n '/hello/Ip' file.txt
```


### Sed Usage

Syntax

```shell
$ sed [options] command [input-file]
```

With pipeline

```sh
$ cat report.txt | sed 's/Nick/John/g'
```

```sh
$ echo '123abc' | sed 's/[0-9]+//g'
```

### Option Examples {.col-span-2}

| Option | Example                                    | Description                             |
| ------ | ------------------------------------------ | --------------------------------------- |
| `-i`   | sed -ibak 's/On/Off/' php.ini              | Backup and modify input file directly   |
| `-E`   | sed -E 's/[0-9]+//g' input-file            | Use extended regular expressions        |
| `-n`   | sed -n '3 p' config.conf                   | Suppress default pattern space printing |
| `-f`   | sed -f script.sed config.conf              | Execute sed script file                 |
| `-e`   | sed -e 'command1' -e 'command2' input-file | Execute multiple sed commands           |

### Multiple commands

```sh {.wrap}
$ echo "hello world" | sed -e 's/h/H/g' -e 's/w/W/g'
Hello World
```

Use `-e` to execute multiple sed commands

### Sed script

```sh
$ echo 's/h/H/g' >> hello.sed
$ echo 's/w/W/g' >> hello.sed
$ echo "hello world" | sed -f hello.sed
Hello World
```

Use `-f` to execute sed script file

### Examples

```sh
$ sed 's/old/new/g' file.txt
$ sed 's/old/new/g' file.txt > new.txt

$ sed 's/old/new/g' -i file.txt
$ sed 's/old/new/g' -i.backup file.txt
```
