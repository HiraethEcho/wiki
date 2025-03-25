---
title: My cheat sheets
toc: true
tags:
  - linux
date: 2024-12-31
---

# My cheat sheets for Linux

## sh

后台挂起

```sh
nohup onedrivegui & > /dev/null
```

## awk

<details><summary>ref</summary>

title: Awk
date: 2020-12-31 15:18:34
background: bg-slate-600
tags:

- bash
- text
- script
  categories:
- Linux Command
  intro: |
  This is a one page quick reference cheat sheet to the [GNU awk](https://www.gnu.org/software/gawk/manual/gawk.html), which covers commonly used awk expressions and commands.
  plugins:
- copyCode

## Getting Started

### Have a try

```shell
$ awk -F: '{print $1, $NF}' /etc/passwd
```

| -             | -                         |
| ------------- | ------------------------- |
| `-F:`         | Colon as a separator      |
| `{...}`       | Awk program               |
| `print`       | Prints the current record |
| `$1`          | First field               |
| `$NF`         | Last field                |
| `/etc/passwd` | Input data file           |

{.left-text}

### Awk program

```
BEGIN          {<initializations>}
   <pattern 1> {<program actions>}
   <pattern 2> {<program actions>}
   ...
END            {< final actions >}
```

#### Example

```
awk '
    BEGIN { print "\n>>>Start" }
    !/(login|shutdown)/ { print NR, $0 }
    END { print "<<<END\n" }
' /etc/passwd
```

### Variables {.row-span-2}

```bash
          $1      $2/$(NF-1)    $3/$NF
           ▼          ▼           ▼
        ┌──────┬──────────────────┬───────┐
$0/NR ▶ │  ID  │  WEBSITE         │  URI  │
        ├──────┼──────────────────┼───────┤
$0/NR ▶ │  1   │  cheatsheets.zip │  awk  │
        ├──────┼──────────────────┼───────┤
$0/NR ▶ │  2   │  google.com      │  25   │
        └──────┴──────────────────┴───────┘
```


```
# First and last field
awk -F: '{print $1,$NF}' /etc/passwd

# With line number
awk -F: '{print NR, $0}' /etc/passwd

# Second last field
awk -F: '{print $(NF-1)}' /etc/passwd

# Custom string
awk -F: '{print $1 "=" $6}' /etc/passwd
```

See: [Variables](#awk-variables)

### Awk program examples {.col-span-2 .row-span-2}

```
awk 'BEGIN {print "hello world"}'        # Prints "hello world"
awk -F: '{print $1}' /etc/passwd         # -F: Specify field separator

# /pattern/ Execute actions only for matched pattern
awk -F: '/root/ {print $1}' /etc/passwd

# BEGIN block is executed once at the start
awk -F: 'BEGIN { print "uid"} { print $1 }' /etc/passwd

# END block is executed once at the end
awk -F: '{print $1} END { print "-done-"}' /etc/passwd
```

### Conditions

```
awk -F: '$3>30 {print $1}' /etc/passwd
```

See: [Conditions](#awk-conditions)

### Generate 1000 spaces

```
awk 'BEGIN{
    while (a++ < 1000)
        s=s " ";
    print s
}'
```

See: [Loops](#awk-loops)

### Arrays

```
awk 'BEGIN {
   fruits["mango"] = "yellow";
   fruits["orange"] = "orange"
   for(fruit in fruits) {
     print "The color of " fruit " is " fruits[fruit]
   }
}'
```

See: [Arrays](#awk-arrays)

### Functions

```
# => 5
awk 'BEGIN{print length("hello")}'
# => HELLO
awk 'BEGIN{print toupper("hello")}'
# => hel
awk 'BEGIN{print substr("hello", 1, 3)}'
```

See: [Functions](#awk-functions)

## Awk Variables

### Build-in variables

| -              | -                                                   |
| -------------- | --------------------------------------------------- |
| `$0`           | Whole line                                          |
| `$1, $2...$NF` | First, second… last field                           |
| `NR`           | `N`umber of `R`ecords                               |
| `NF`           | `N`umber of `F`ields                                |
| `OFS`          | `O`utput `F`ield `S`eparator <br> _(default " ")_   |
| `FS`           | input `F`ield `S`eparator <br> _(default " ")_      |
| `ORS`          | `O`utput `R`ecord `S`eparator <br> _(default "\n")_ |
| `RS`           | input `R`ecord `S`eparator <br> _(default "\n")_    |
| `FILENAME`     | Name of the file                                    |

### Expressions

| -                   | -                                  |
| ------------------- | ---------------------------------- |
| `$1 == "root"`      | First field equals root            |
| `{print $(NF-1)}`   | Second last field                  |
| `NR!=1{print $0}`   | From 2nd record                    |
| `NR > 3`            | From 4th record                    |
| `NR == 1`           | First record                       |
| `END{print NR}`     | Total records                      |
| `BEGIN{print OFMT}` | Output format                      |
| `{print NR, $0}`    | Line number                        |
| `{print NR "	" $0}`  | Line number (tab)                  |
| `{$1 = NR; print}`  | Replace 1st field with line number |
| `$NF > 4`           | Last field > 4                     |
| `NR % 2 == 0`       | Even records                       |
| `NR==10, NR==20`    | Records 10 to 20                   |
| `BEGIN{print ARGC}` | Total arguments                    |
| `ORS=NR%5?",":"\n"` | Concatenate records                |

### Examples

Print sum and average

```
awk -F: '{sum += $3}
     END { print sum, sum/NR }
' /etc/passwd
```

Printing parameters

```
awk 'BEGIN {
    for (i = 1; i < ARGC; i++)
        print ARGV[i] }' a b c
```

Output field separator as a comma

```
awk 'BEGIN { FS=":";OFS=","}
    {print $1,$2,$3,$4}' /etc/passwd
```

Position of match

```
awk 'BEGIN {
    if (match("One Two Three", "Tw"))
        print RSTART }'
```

Length of match

```
awk 'BEGIN {
    if (match("One Two Three", "re"))
        print RLENGTH }'
```

### Environment Variables

| -         | -                                                         |
| --------- | --------------------------------------------------------- |
| `ARGC`    | Number or arguments                                       |
| `ARGV`    | Array of arguments                                        |
| `FNR`     | `F`ile `N`umber of `R`ecords                              |
| `OFMT`    | Format for numbers <br> _(default "%.6g")_                |
| `RSTART`  | Location in the string                                    |
| `RLENGTH` | Length of match                                           |
| `SUBSEP`  | Multi-dimensional array separator <br> _(default "\034")_ |
| `ARGIND`  | Argument Index                                            |

### GNU awk only

| -             | -                     |
| ------------- | --------------------- |
| `ENVIRON`     | Environment variables |
| `IGNORECASE`  | Ignore case           |
| `CONVFMT`     | Conversion format     |
| `ERRNO`       | System errors         |
| `FIELDWIDTHS` | Fixed width fields    |

### Defining variable

```
awk -v var1="Hello" -v var2="Wold" '
    END {print var1, var2}
' </dev/null
```

#### Use shell variables

```
awk -v varName="$PWD" '
    END {print varName}' </dev/null
```

## Awk Operators

### Operators

| -                | -           |
| ---------------- | ----------- |
| `{print $1}`     | First field |
| `$2 == "foo"`    | Equals      |
| `$2 != "foo"`    | Not equals  |
| `"foo" in array` | In array    |

#### Regular expression

| -               | -                 |
| --------------- | ----------------- |
| `/regex/`       | Line matches      |
| `!/regex/`      | Line not matches  |
| `$1 ~ /regex/`  | Field matches     |
| `$1 !~ /regex/` | Field not matches |

#### More conditions

| -                        | -   |
| ------------------------ | --- |
| `($2 <= 4 \|\| $3 < 20)` | Or  |
| `($1 == 4 && $3 < 20)`   | And |

### Operations

#### Arithmetic operations

- `+`
- `-`
- `*`
- `/`
- `%`
- `++`
- `--`

{.cols-3 .marker-none}

#### Shorthand assignments

- `+=`
- `-=`
- `*=`
- `/=`
- `%=`

{.cols-3 .marker-none}

#### Comparison operators

- `==`
- `!=`
- `<`
- `>`
- `<=`
- `>=`

{.cols-3 .marker-none}

### Examples

```
awk 'BEGIN {
    if ("foo" ~ "^fo+$")
        print "Fooey!";
}'
```

#### Not match

```
awk 'BEGIN {
    if ("boo" !~ "^fo+$")
        print "Boo!";
}'
```

#### if in array

```
awk 'BEGIN {
    assoc["foo"] = "bar";
    assoc["bar"] = "baz";
    if ("foo" in assoc)
        print "Fooey!";
}'
```

## Awk Functions

### Common functions {.col-span-2}

| Function              | Description                                                                     |
| --------------------- | ------------------------------------------------------------------------------- |
| `index(s,t)`          | Position in string s where string t occurs, 0 if not found                      |
| `length(s)`           | Length of string s (or $0 if no arg)                                            |
| `rand`                | Random number between 0 and 1                                                   |
| `substr(s,index,len)` | Return len-char substring of s that begins at index (counted from 1)            |
| `srand`               | Set seed for rand and return previous seed                                      |
| `int(x)`              | Truncate x to integer value                                                     |
| `split(s,a,fs)`       | Split string s into array a split by fs, returning length of a                  |
| `match(s,r)`          | Position in string s where regex r occurs, or 0 if not found                    |
| `sub(r,t,s)`          | Substitute t for first occurrence of regex r in string s (or $0 if s not given) |
| `gsub(r,t,s)`         | Substitute t for all occurrences of regex r in string s                         |
| `system(cmd)`         | Execute cmd and return exit status                                              |
| `tolower(s)`          | String s to lowercase                                                           |
| `toupper(s)`          | String s to uppercase                                                           |
| `getline`             | Set $0 to next input record from current input file.                            |

### User defined function

```
awk '
    # Returns minimum number
    function find_min(num1, num2){
       if (num1 < num2)
       return num1
       return num2
    }
    # Returns maximum number
    function find_max(num1, num2){
       if (num1 > num2)
       return num1
       return num2
    }
    # Main function
    function main(num1, num2){
       result = find_min(num1, num2)
       print "Minimum =", result

       result = find_max(num1, num2)
       print "Maximum =", result
    }
    # Script execution starts here
    BEGIN {
       main(10, 60)
    }
'
```

## Awk Arrays

### Array with index

```
awk 'BEGIN {
    arr[0] = "foo";
    arr[1] = "bar";
    print(arr[0]); # => foo
    delete arr[0];
    print(arr[0]); # => ""
}'
```

### Array with key

```
awk 'BEGIN {
    assoc["foo"] = "bar";
    assoc["bar"] = "baz";
    print("baz" in assoc); # => 0
    print("foo" in assoc); # => 1
}'
```

### Array with split

```
awk 'BEGIN {
    split("foo:bar:baz", arr, ":");
    for (key in arr)
        print arr[key];
}'
```

### Array with asort

```
awk 'BEGIN {
    arr[0] = 3
    arr[1] = 2
    arr[2] = 4
    n = asort(arr)
    for (i = 1; i <= n ; i++)
        print(arr[i])
}'
```

### Multi-dimensional

```
awk 'BEGIN {
    multidim[0,0] = "foo";
    multidim[0,1] = "bar";
    multidim[1,0] = "baz";
    multidim[1,1] = "boo";
}'
```

### Multi-dimensional iteration

```
awk 'BEGIN {
    array[1,2]=3;
    array[2,3]=5;
    for (comb in array) {
        split(comb,sep,SUBSEP);
        print sep[1], sep[2],
        array[sep[1],sep[2]]
    }
}'
```

## Awk Conditions

### if-else statement

```
awk -v count=2 'BEGIN {
    if (count == 1)
        print "Yes";
    else
        print "Huh?";
}'
```

#### Ternary operator

```
awk -v count=2 'BEGIN {
    print (count==1) ? "Yes" : "Huh?";
}'
```

### Exists

```
awk 'BEGIN {
    assoc["foo"] = "bar";
    assoc["bar"] = "baz";
    if ("foo" in assoc)
        print "Fooey!";
}'
```

#### Not exists

```
awk 'BEGIN {
    assoc["foo"] = "bar";
    assoc["bar"] = "baz";
    if ("Huh" in assoc == 0 )
        print "Huh!";
}'
```

### switch

```
awk -F: '{
    switch (NR * 2 + 1) {
        case 3:
        case "11":
            print NR - 1
            break

        case /2[[:digit:]]+/:
            print NR

        default:
            print NR + 1

        case -1:
            print NR * -1
    }
}' /etc/passwd
```

## Awk Loops

### for...i

```
awk 'BEGIN {
    for (i = 0; i < 10; i++)
        print "i=" i;
}'
```

#### Powers of two between 1 and 100

```
awk 'BEGIN {
    for (i = 1; i <= 100; i *= 2)
        print i
}'
```

### for...in

```
awk 'BEGIN {
    assoc["key1"] = "val1"
    assoc["key2"] = "val2"
    for (key in assoc)
        print assoc[key];
}'
```

#### Arguments

```
awk 'BEGIN {
    for (argnum in ARGV)
        print ARGV[argnum];
}' a b c
```

### Examples {.row-span-3}

#### Reverse records

```
awk -F: '{ x[NR] = $0 }
    END {
        for (i = NR; i > 0; i--)
        print x[i]
    }
' /etc/passwd
```

#### Reverse fields

```
awk -F: '{
    for (i = NF; i > 0; i--)
        printf("%s ",$i);
    print ""
}' /etc/passwd
```

#### Sum by record

```
awk -F: '{
    s=0;
    for (i = 1; i <= NF; i++)
        s += $i;
    print s
}' /etc/passwd
```

#### Sum whole file

```
awk -F: '
    {for (i = 1; i <= NF; i++)
        s += $i;
    };
    END{print s}
' /etc/passwd
```

### while {.row-span-2}

```
awk 'BEGIN {
    while (a < 10) {
        print "- " " concatenation: " a
        a++;
    }
}'
```

#### do...while

```
awk '{
    i = 1
    do {
        print $0
        i++
    } while (i <= 5)
}' /etc/passwd
```

### Break

```
awk 'BEGIN {
    break_num = 5
    for (i = 0; i < 10; i++) {
        print i
        if (i == break_num)
            break
    }
}'
```

### Continue

```
awk 'BEGIN {
    for (x = 0; x <= 10; x++) {
        if (x == 5 || x == 6)
            continue
        printf "%d ", x
    }
    print ""
}'
```

## Awk Formatted Printing

### Usage

#### Right align

```
awk 'BEGIN{printf "|%10s|\n", "hello"}'

|     hello|
```

#### Left align

```
awk 'BEGIN{printf "|%-10s|\n", "hello"}'

|hello     |
```

### Common specifiers

| Character     | Description           |
| ------------- | --------------------- |
| `c`           | ASCII character       |
| `d`           | Decimal integer       |
| `e`, `E`, `f` | Floating-point format |
| `o`           | Unsigned octal value  |
| `s`           | String                |
| `%`           | Literal %             |

### Space

```
awk -F: '{
    printf "%-10s %s\n", $1, $(NF-1)
}' /etc/passwd | head -n 3
```

Outputs

```shell script
root       /root
bin        /bin
daemon     /sbin
```

### Header

```
awk -F: 'BEGIN {
    printf "%-10s %s\n", "User", "Home"
    printf "%-10s %s\n", "----","----"}
    { printf "%-10s %s\n", $1, $(NF-1) }
' /etc/passwd | head -n 5
```

Outputs

```
User       Home
----       ----
root       /root
bin        /bin
daemon     /sbin
```

## Miscellaneous

### Regex Metacharacters

- `\`
- `^`
- `$`
- `.`
- `[`
- `]`
- `|`
- `(`
- `)`
- `*`
- `+`
- `?`


### Escape Sequences

| -    | -                   |
| ---- | ------------------- |
| `\b` | Backspace           |
| `\f` | Form feed           |
| `\n` | Newline (line feed) |
| `\r` | Carriage return     |
| `\t` | Horizontal tab      |
| `\v` | Vertical tab        |

### Run script

```shell script
$ cat demo.awk
#!/usr/bin/awk -f
BEGIN { x = 23 }
      { x += 2 }
END   { print x }
$ awk -f demo.awk /etc/passwd
69
```

## Also see

- [The GNU Awk User's Guide](https://www-zeuthen.desy.de/dv/documentation/unixguide/infohtml/gawk/gawk.html)
  _(www-zeuthen.desy.de)_
- [AWK cheatsheet](https://gist.github.com/Rafe/3102414) _(gist.github.com)_

</details>

> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [www.hackingnote.com](https://www.hackingnote.com/en/cheatsheets/awk/)

> HackingNoteHackingNote

## Built-in

- `$1`: `awk` reads and parses each line from input based on _whitespace_ character by default and set the variables `$1`, `$2` and etc.
- `NF`: Number of Fields.
- `NR`: Number of Records.
- `FNR`: Number of Records relative to the current input file.
- `FS`: Field Separator, any single character or regular expression, instead of the default _whitespace_. `,` in the script will be replaced with the field separator.
- `OFS`: Output Field Separator.
- `RS`: Record Separator.
- `ORS`: Output Record Separator.
- `FILENAME`: Name of the current input file.

## Count Columns

If delimiter is `,`

```
$ cat foo.txt | awk -F, '{print NF}'



```

or

```
$ awk 'BEGIN {FS=","} {print NF}' file.txt



```

If delimiter is `\u0007`(`ctrl-v ctrl-g`)

```
$ cat foo.txt | awk -F'^G' '{print NF}'



```

## Get Column Number

replace `<pattern>` with the column name or pattern

```
head -1 foo.csv | awk -v RS="|" '/<pattern>/{print NR;}'



```

## Print Rows by Number

print the second row:

```
$ awk 'NR==2' filename



```

print line 2 to line 10

```
$ awk 'NR==2,NR==10' filename



```

## FS

`FS` can be set in either of these ways:

- Using -F command line option. `awk -F 'FS' 'commands' inputfilename`
- Awk FS can be set like normal variable. `awk 'BEGIN{FS="FS";}'`

Example to read the `/etc/passwd` file which has `:` as field delimiter.

```
$ cat etc_passwd.awk
BEGIN{
    FS=":";
    print "Name\tUserID\tGroupID\tHomeDirectory";
}
{
    print $1"\t"$3"\t"$4"\t"$6;
}
END {
    print NR,"Records Processed";
}



```

Then

```
$awk -f etc_passwd.awk /etc/passwd
Name UserID GroupID HomeDirectory
gnats 41 41 /var/lib/gnats
libuuid 100 101 /var/lib/libuuid
syslog 101 102 /home/syslog
hplip 103 7 /var/run/hplip
avahi 105 111 /var/run/avahi-daemon
saned 110 116 /home/saned
pulse 111 117 /var/run/pulse
gdm 112 119 /var/lib/gdm
8 Records Processed



```

## OFS

Similar to `FS` but for outputs.

```
$ awk -F':' '{print $3,$4;}' /etc/passwd
41 41
100 101
101 102


$ awk -F':' 'BEGIN{OFS="=";} {print $3,$4;}' /etc/passwd
41=41
100=101
101=102



```

## RS

`awk` reads a line as a "record" by default. To split the text into records differently, set `RS`. E.g. if `input.txt` is like this:

```
A
1
101

B
2
202

C
3
303



```

Set `RS` to double new line characters (`\n\n`) so the input text is split into 3 records:

```
$cat script.awk
BEGIN {
    RS="\n\n";
    FS="\n";
}
{
    print $1,$2;
}

$ awk -f script.awk input.txt
A 1
B 2
C 3



```

## ORS

The output equivalent of `RS`. The records will be printed with `ORS` as the separator instead of the default new line.

```
$ echo "A 1\nB 2\nC 3" | awk 'BEGIN{ORS="="}{print;}'
A 1=B 2=C 3=%



```

## Extract a Column

```
$ cat file | awk '{print $2}'



```

## Add awk in alias

```
awk '{print $1}'
alias aprint='awk "{print \$1}"'



```

## Pattern Matching

```
... | awk '/pattern/'


... | awk '/pattern/ {print $1}'



```

## sed

<details><summary>quickref</summary>

## Getting Started

### Sed Usage

Syntax

```shell script
$ sed [options] command [input-file]
```

With pipeline

```shell script
$ cat report.txt | sed 's/Nick/John/g'
```

```shell script
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

{.show-header}

### Multiple commands

```shell script {.wrap}
$ echo "hello world" | sed -e 's/h/H/g' -e 's/w/W/g'
Hello World
```

Use `-e` to execute multiple sed commands

### Sed script

```shell script
$ echo 's/h/H/g' >> hello.sed
$ echo 's/w/W/g' >> hello.sed
$ echo "hello world" | sed -f hello.sed
Hello World
```

Use `-f` to execute sed script file

### Examples

```shell script
$ sed 's/old/new/g' file.txt
$ sed 's/old/new/g' file.txt > new.txt

$ sed 's/old/new/g' -i file.txt
$ sed 's/old/new/g' -i.backup file.txt
```

See: [Sed examples](#sed-examples)

## Sed commands

### Commands {.col-span-2}

| Command | Example                                | Description                 |
| ------- | -------------------------------------- | --------------------------- |
| `p`     | sed -n '1,4 p' input.txt               | Print lines 1-4             |
| `p`     | sed -n -e '1,4 p' -e '6,7 p' input.txt | Print lines 1-4 and 6-7     |
| `d`     | sed '1,4 d' input.txt                  | Print lines except 1-4      |
| `w`     | sed -n '1,4 w output.txt' input.txt    | Write pattern space to file |
| `a`     | sed '2 a new-line' input.txt           | Append line after           |
| `i`     | sed '2 i new-line' input.txt           | Insert line before          |

{.show-header}

### Space commands

| Command | Description                                                  |
| ------- | ------------------------------------------------------------ |
| `n`     | Print pattern space, empty pattern space, and read next line |
| `x`     | Swap pattern space with hold space                           |
| `h`     | Copy pattern space to hold space                             |
| `H`     | Append pattern space to hold space                           |
| `g`     | Copy hold space to pattern space                             |
| `G`     | Append hold space to pattern space                           |

See also: [File spacing](#file-spacing)

### Flags

```shell script
$ sed 's/old/new/[flags]' [input-file]
```

---

| Flag     | Description                                |
| -------- | ------------------------------------------ |
| `g`      | Global substitution                        |
| `1,2...` | Substitute the nth occurrence              |
| `p`      | Print only the substituted line            |
| `w`      | Write only the substituted line to a file  |
| `I`      | Ignore case while searching                |
| `e`      | Substitute and execute in the command line |

### Loops commands

| Command   | Description                                                        |
| --------- | ------------------------------------------------------------------ |
| `b label` | Branch to a label (for looping)                                    |
| `t label` | Branch to a label only on successful substitution<br>(for looping) |
| `:label`  | Label for the b and t commands (for looping)                       |
| `N`       | Append next line to pattern space                                  |
| `P`       | Print 1st line in multi-line                                       |
| `D`       | Delete 1st line in multi-line                                      |

### Misc Flags

| Flag                      | Description                                                                  |
| ------------------------- | ---------------------------------------------------------------------------- |
| <code>/ \| ^ @ ! #</code> | Substitution delimiter can be any character                                  |
| `&`                       | Gets the matched pattern                                                     |
| `( ) \1 \2 \3`            | Group using `(` and `)`.<br>Use `\1`, `\2` in replacement to refer the group |

## Sed examples

### Replacing text {.row-span-2}

Replace all occurrences of a string

```shell script
$ sed 's/old/new/g' file.txt
```

Replace only the nth occurrence of a string

```shell script
$ sed 's/old/new/2' file.txt
```

Replace a string only on the 5th line

```shell script
$ sed '5 s/old/new/' file.txt
```

Replace "world" with "universe" but only if the line begins with "hello"

```shell script
$ sed '/hello/s/world/universe/' file.txt
```

Remove "\" from the end of each line

```shell script
$ sed 's/\\$//' file.txt
```

Remove all whitespace from beginning of each line

```shell script
$ sed 's/^\s*//' file.txt
```

Remove comments. Even those that are at the end of a line

```shell script
$ sed 's/#.*$//' file.txt
```

### Search for text

Search for a string and only print the lines that were matched

```shell script
$ sed -n '/hello/p' file.txt
```

Case insensitive search

```shell script
$ sed -n '/hello/Ip' file.txt
```

Search for a string but only output lines that do not match

```shell script
$ sed -n '/hello/!p' file.txt
```

### Appending lines

Append line after line 2

```shell script
$ sed '2a Text after line 2' file.txt
```

Append line at the end of the file

```shell script
$ sed '$a THE END!' file.txt
```

Append line after every 3rd line starting from line 3

```shell script
$ sed '3~3a Some text' file.txt
```

### Numbering {.col-span-2}

Number line of a file (simple left alignment)

```shell script
$ sed = file.txt | sed 'N;s/\n/\t/'
```

Number line of a file (number on left, right-aligned)

```shell script
$ sed = file.txt | sed 'N; s/^/   /; s/ *\(.\{6,\}\)\n/\1  /'
```

Number line of file, but only print numbers if line is not blank

```shell script
$ sed '/./=' file.txt | sed '/./N; s/\n/ /'
```

Count lines (emulates "wc -l")

```shell script
$ sed -n '$='
```

### Prepending lines

Insert text before line 5

```shell script
$ sed '5i line number five' file.txt
```

Insert "Example: " before each line that contains "hello"

```shell script
$ sed '/hello/i Example: ' file.txt
```

### Deleting lines

Delete line 5-7 in file

```shell script
$ sed '5,7d' file.txt
```

Delete every 2nd line starting with line 3

```shell script
$ sed '3~2d' file.txt
```

Delete the last line in file

```shell script
$ sed '$d' file.txt
```

Delete lines starting with "Hello"

```shell script
$ sed '/^Hello/d' file.txt
```

Delete all empty lines

```shell script
$ sed '/^$/d' file.txt
```

Delete lines starting with "#"

```shell script
$ sed '/^#/d' file.txt
```

### File spacing

Double space

```shell script
$ sed G
```

Delete all blank lines and double space

```shell script
$ sed '/^$/d;G'
```

Triple space a file

```shell script
$ sed 'G;G'
```

Undo double-spacing

```shell script
$ sed 'n;d'
```

Insert a blank line above line which matches "regex"

```shell script
$ sed '/regex/{x;p;x;}'
```

Insert a blank line below line which matches "regex"

```shell script
$ sed '/regex/G'
```

Insert a blank line around line which matches "regex"

```shell script
$ sed '/regex/{x;p;x;G;}'
```

## Also see {.cols-1}

- [sed cheatsheet](https://gist.github.com/ssstonebraker/6140154) _(gist.github.com)_
</details>

> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [quickref.me](https://quickref.me/sed.html)

> Sed is a stream editor, this sed cheat sheet contains sed commands and some common sed tricks.

### [#](#replacing-text)Replacing text

Replace all occurrences of a string

```
$ sed 's/old/new/g' file.txt


```

Replace only the nth occurrence of a string

```
$ sed 's/old/new/2' file.txt


```

Replace replace a string only on the 5th line

```
$ sed '5 s/old/new/' file.txt


```

Replace "world" with "universe" but only if the line begins with "hello"

```
$ sed '/hello/s/world/universe/' file.txt


```

Remove "" from the end of each line

```
$ sed 's/\\$//' file.txt


```

Remove all whitespace from beginning of each line

```
$ sed 's/^\s*//' file.txt


```

Remove comments. Even those that are at the end of a line

```
$ sed 's/#.*$//' file.txt


```

### [#](#search-for-text)Search for text

Search for a string and only print the lines that were matched

```
$ sed -n '/hello/p' file.txt


```

Case insensitive search

```
$ sed -n '/hello/Ip' file.txt


```

Search for a string but only output lines that do not match

```
$ sed -n '/hello/!p' file.txt


```

### [#](#appending-lines)Appending lines

Append line after line 2

```
$ sed '2a Text after line 2' file.txt


```

Append line at the end of the file

```
$ sed '$a THE END!' file.txt


```

Append line after every 3rd line starting from line 3

```
$ sed '3~3a Some text' file.txt


```

### [#](#numbering)Numbering

Number line of a file (simple left alignment)

```
$ sed = file.txt | sed 'N;s/\n/\t/'


```

Number line of a file (number on left, right-aligned)

```
$ sed = file.txt | sed 'N; s/^/   /; s/ *\(.\{6,\}\)\n/\1  /'


```

Number line of file, but only print numbers if line is not blank

```
$ sed '/./=' file.txt | sed '/./N; s/\n/ /'


```

Count lines (emulates "wc -l")

```
$ sed -n '$='


```

### [#](#prepending-lines)Prepending lines

Insert text before line 5

```
$ sed '5i line number five' file.txt


```

Insert "Example:" before each line that contains "hello"

```
$ sed '/hello/i Example: ' file.txt


```

### [#](#deleting-lines)Deleting lines

Delete line 5-7 in file

```
$ sed '5,7d' file.txt


```

Delete every 2nd line starting with line 3

```
$ sed '3~2d' file.txt


```

Delete the last line in file

```
$ sed '$d' file.txt


```

Delete lines starting with "Hello"

```
$ sed '/^Hello/d' file.txt


```

Delete all empty lines

```
$ sed '/^$/d' file.txt


```

Delete lines starting with "#"

```
$ sed '/^#/d' file.txt


```

### [#](#file-spacing)File spacing

Double space

```
$ sed G


```

Delete all blank lines and double space

```
$ sed '/^$/d;G'


```

Triple space a file

```
$ sed 'G;G'


```

Undo double-spacing

```
$ sed 'n;d'


```

Insert a blank line above line which matches "regex"

```
$ sed '/regex/{x;p;x;}'


```

Insert a blank line below line which matches "regex"

```
$ sed '/regex/G'


```

Insert a blank line around line which matches "regex"

```
$ sed '/regex/{x;p;x;G;}'


```

## grep

> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [www.hackingnote.com](https://www.hackingnote.com/en/cheatsheets/grep/)

> HackingNote

## Count Occurrence: `-c`

```
$ cat example.txt | grep -c good
2


```

## Get Context: `-C`

Set `--context=0` to print that line alone

```
$ cat example.txt | grep --context=0 "good luck"
good luck


```

Set `--context=1` to print 1 line below and 1 line above

```
$ cat example.txt | grep --context=1 "good luck"
hello world
good luck
good day


```

or use `-C 1`

```
$ cat example.txt | grep -C 1 "good luck"
hello world
good luck
good day


```

## Ignore: `-v`

Use `-v` to exclude some lines(i.e. NOT)

```
$ cat example.txt | grep good | grep -v day
good luck


```

## Case Insensitive: `-i`

```
$ cat example.txt | grep GOOD

$ cat example.txt | grep -i GOOD
good luck
good day


```

## Show Match in Color: `--color`

```
$ cat example.txt | grep good --color


```

## Show Matched Line Number: `-n`

```
$ cat example.txt | grep good -n
2:good luck
3:good day


```

## Show Matched File Name: `-l`

`grep` is not limited to searching a single file, compare the results below

```
$ grep good example.txt
good luck
good day


```

to search from multiple files:

```
$ grep good *
example.txt:good luck
example.txt:good day


```

filename will be shown along with the matched lines; to show the filename only:

```
$ grep -l good *
example.txt


```

what happens to the "pipe" version?

```
$ cat example.txt | grep good -l
(standard input)


```

## Search for Whole Words Only: `-w`

```
$ grep -w goo example.txt


```

this returns nothing since `goo` is a pattern though not a whole word

```
$ grep goo example.txt
good luck
good day


```

## Recursive grep: `-R`

This will search all the directory and sub-directories recursively

```
$ grep -R pattern *


```

## Set Maximum Matches: `-m`

```
$ cat example.txt | grep -m 1 good
good luck


```

## Match

Show `ssh` processes

```
$ ps | grep ssh


```

Specify max count by `-m`:

```
$ cat foo.log | grep -m 10 ERROR


```

## Show file name

```
grep -H


```

## grep vs egrap vs fgrep

`grep`, `egrep` and `fgrep` are used to match patterns in files, here are the differences:

- `grep`: basic regular expressions
- `egrep`: extended regular expressions(`?`, `+`, `|`), equivalent to `grep -E`
- `fgrep`: fixed patterns, no regular expression; faster than grep and egrep; equivalent to `grep -F`

Checkout the [Regular_expression wikipedia page](https://en.wikipedia.org/wiki/Regular_expression#POSIX_basic_and_extended) for the definitions of POSIX basic and extended regular expressions

Assume there's a `examle.txt` file containing 4 lines:

```
$ cat example.txt
hello world
good luck
good day
linux


```

### `grep` vs `fgrep`

`fgrep` does not support regular expression at all, this will return nothing

```
$ cat example.txt | fgrep g..d


```

use `grep` instead

```
$ cat example.txt | grep g..d
good luck
good day


```

### `grep` vs `egrep`

`grep` does not support `|`, so this will return nothing

```
$ cat example.txt | grep "good|linux"


```

however `egrep` can recognize `|` as OR

```
$ cat example.txt | egrep "good|linux"
good luck
good day
linux


```

## cut

## git

## regex

> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [quickref.me](https://quickref.me/regex)

> A quick reference for regular expressions (regex), including symbols, ranges, grouping, assertions an......

#### [#](#re-search)re.search()

```
>>> sentence = 'This is a sample string'
>>> bool(re.search(r'this', sentence, flags=re.I))
True
>>> bool(re.search(r'xyz', sentence))
False


```

#### [#](#re-findall)re.findall()

```
>>> re.findall(r'\bs?pare?\b', 'par spar apparent spare part pare')
['par', 'spar', 'spare', 'pare']
>>> re.findall(r'\b0*[1-9]\d{2,}\b', '0501 035 154 12 26 98234')
['0501', '154', '98234']


```

#### [#](#re-finditer)re.finditer()

```
>>> m_iter = re.finditer(r'[0-9]+', '45 349 651 593 4 204')
>>> [m[0] for m in m_iter if int(m[0]) < 350]
['45', '349', '4', '204']


```

#### [#](#re-split)re.split()

```
>>> re.split(r'\d+', 'Sample123string42with777numbers')
['Sample', 'string', 'with', 'numbers']


```

#### [#](#re-sub)re.sub()

```
>>> ip_lines = "catapults\nconcatenate\ncat"
>>> print(re.sub(r'^', r'* ', ip_lines, flags=re.M))
* catapults
* concatenate
* cat


```

#### [#](#re-compile)re.compile()

```
>>> pet = re.compile(r'dog')
>>> type(pet)
<class '_sre.SRE_Pattern'>
>>> bool(pet.search('They bought a dog'))
True
>>> bool(pet.search('A cat crossed their path'))
False


```

> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [www.hackingnote.com](https://www.hackingnote.com/en/cheatsheets/regex/)

> HackingNote

## Syntax

- `|`: or
- `()`: group

### Characters

- `.`: any character (dot matches everything except newlines)
- `\w`: alphanumeric character plus `_`, equivalent to `[A-Za-z0-9_]`
- `\W`: non-alphanumeric character excluding `_`, equivalent to `[^A-Za-z0-9_]`
- `\s`: whitespace
- `\S`: anything BUT whitespace
- `\d`: digit, equivalent to `[0-9]`
- `\D`: non-digit, equivalent to `[^0-9]`
- `[...]`: one of the characters
- `[^...]`: anything but the characters listed

### Anchors

- `^`: beginning of a line or string
- `$`: end of a line or string
- `\b`: zero-width word-boundary (like the caret and the dollar sign)
- `\A`: Matches the beginning of a string (but not an internal line).
- `\z`: Matches the end of a string (but not an internal line).

### Repetition Operators

- `?`: match 0 or 1 times
- `+`: match at least once
- `*`: match 0 or multiple times
- `{M,N}`: minimum M matches and maximum N matches
  - `{M,}`: match at least M times
  - `{0,N}`: match at most N times

## Greedy vs Lazy

- `.*`: match as long as possible
- `.*?`: match as short as possible

## BRE vs ERE vs PCRE

The only difference between basic and extended regular expressions is in the behavior of a few characters: `?`, `+`, parentheses (`()`), and braces (`{}`).

- **basic regular expressions (BRE)**: should be escaped to behave as special characters
- **extended regular expressions (ERE)** : should be escaped to match a literal character.
- **Perl Compatible Regular Expressions (PCRE)**: much more powerful and flexible than BRE and ERE.

Multiple flavors may be supported by the tools:

- sed
  - `sed`: basic
  - `sed -E`: extended
- grep
  - `grep`: basic
  - `egrep` or `grep -E`

## JavaScript

- `str.search`
- `str.match`
- `str.matchAll`
- `str.replace`

Example: split country name and country code in strings like "China (CN)"

```
> s = "China (CN)";
'China (CN)'
> match = s.match(/\((.*?)\)/)
[ '(CN)', 'CN', index: 6, input: 'China (CN)', groups: undefined ]
> match[1]
'CN'
> s.substring(0, match.index).trim()
'China'


```

Match all:

```
const regex = /.*/g;
const matches = content.matchAll(regex);
for (let match of matches) {


}


```

### Literal vs. Constructor

- Literal: `re = /.../g`
- Constructor: `re = new RegExp("...")`
  - can use string concat: `re = new RegExp("..." + some_variable + "...")`

### Local vs. Global

- `re = /.../`: `re.match(str)` will return a list of captures of the FIRST match.
- `re = /.../g`: `re.match(str)` will return a list of matches but NOT captures.

### match vs. exec

- `str.match()`: as stated above.
- `regex.exec()`: return captures, more detailed info; exec multiple times.

Example

```
var match;
while ((match = re.exec(str)) !== null) {}


```

## Python

`match`, `search` and `findall`:

- `re.match()`: only match at the beginning of the string, returns a `match` object.
- `re.search()`: locate a match anywhere in string, returns a `match` object.
- `re.findall()`: find all occurrences, returns a list of strings.

```
>>> type(re.search("foo", "foobarfoo"))
<class '_sre.SRE_Match'>
>>> type(re.match("foo", "foobarfoo"))
<class '_sre.SRE_Match'>


```

### re.match()/re.search()

`re.match()` and `re.search()` return a `match` object:

```
>>> match = re.search("f(.*?),", "foo,faa,fuu,bar")
>>> match.groups()
('oo',)


```

`match.group(0)` returns the string snippet that matches the pattern:

```
>>> match.group(0)
'foo,'


```

other `group` captures the ones in `()`:

```
>>> match.group(1)
'oo'


```

### re.findall()

`re.findall()` returns a list, extract value using `[]`:

```
>>> match = re.findall("f(.*?),", "foo,faa,fuu,bar")
>>> match
['oo', 'aa', 'uu']
>>> match[0]
'oo'


```

### Compiled Patterns

```
pattern = re.compile(pattern_string)
result = pattern.match(string)


```

is equivalent to

```
result = re.match(pattern_string, string)


```

`re.compile()` returns a `SRE_Pattern` object:

```
>>> type(re.compile("pattern"))
<class '_sre.SRE_Pattern'>


```
