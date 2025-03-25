---
title: Bash Scripts
toc: true
tags:
  - linux
date: 2024-12-31
---

# Bash Scripts

## get-opts

example:

```sh
#!/bin/sh
docs="Usage: \
\n\tbash $0 [options] \
\nOptions: \
\n\t-N NAME: your name, required\
\n\t-a AGE: your age, required\
\n\t-v: verbose mode, optional\
\nExample: \
\n\t bash $0 -N balabala -a 24 "

usage() {
    echo -e $docs >&2
    exit 1
}

if [ $# -eq 0 ] || [ $1 == -h ]; then usage; fi

## 设置变量的默认值
verbose=n

## 检查变量参数是否缺失
check() {
    opt=$1
    arg=$2

    if [[ $arg =~ ^- ]] || [ ! $arg ]
    then
        echo "ERROR: -$opt expects an corresponding argument" >&2
        usage
    fi
}

## 循环解析脚本的所有positional parameters
while getopts :vN:a: opt
do
    case $opt in
    v)
        verbose=y
        ;;
    N)
        check N $OPTARG
        name=$OPTARG
        ;;
    a)
        check a $OPTARG
        age=$OPTARG
        ;;
    :)
        echo "ERROR: -$OPTARG expects an corresponding argument" >&2
        usage
        ;;
    \?) # shell里'?'具有匹配一位任意字符的作用，因此需要转义为普通字符
        echo "ERROR: unkown option -$OPTARG" >&2
        usage
        ;;
    esac
done

## 如果用户没有提供-N选项
if [ ! $name ]
then
    echo ERROR: option -N is required >&2
    usage
fi

## 如果用户没有提供-a选项
if [ ! $age ]
then
    echo ERROR: option -a is required >&2
    usage
fi

if [ $verbose == n ]
then
    echo "Merry Christmas! $name"
else
    echo "Hello, your name is $name, you are $age years old!"
    echo "Merry Christmas!!"
fi
```
