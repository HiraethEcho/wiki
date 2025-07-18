---
title: "Esoteric Vim"
source: "https://freestingo.com/en/programming/articles/esoteric-vim/"
date: 2025-05-26
description: "A list of niche Vim features and their time-saving, real-life applications"
dg-publish: true
---
This is a compendium of useful yet lesser-known Vim idioms I actually use in my everyday editing, saving me dozens (if not hundreds) of hours of work. The article is meant both as a resource for the curious vimmer wanting to expand their vocabulary, and as an answer to the question puzzled non-vimmers ask themselves whenever they accidentally open Vim and cannot figure out how to close it: why in the world do people even bother learning this weird text editor in the first place?

## 1\. Assigning numbers to existing enum classes

Here is an example of a C# enum class:

```csharp
1public enum TranslationCodeId
 2{
 3   // buttons
 4   BUTTON_Accept,
 5   BUTTON_Cancel,
 6
 7   // labels
 8   LABEL_Actions,
 9   LABEL_Column,
10
11   // messages
12   MESSAGE_Confirm,
13   MESSAGE_GoBack,
14
15   // more fields...
16}
```

Its current state is potentially problematic, though. Implicit value enum members could allow programmers to silently add new ones in the middle, implicitly changing the value of all subsequent values as well and creating discrepancies if any of them were also being persisted somewhere else in their numeric form. One way to prevent this is to add explicit values next to all members. It is after all a very easy edit, but it can quickly become a nightmare if your enum does not contain six fields only, but, say, 800. A manual update of every single line would simply be unfeasible, or at the very least a painfully boring and time-consuming task. In Vim, you can do it all in two steps:

### 1.1. Assign 0 to all enum fields

```diff
:g/,$/v;//;norm $i = 0
```
- `:g` is a [`global`](https://vimhelp.org/repeat.txt.html#%3Aglobal "'global' on vimhelp.org") command: it executes any given action on all matching lines. `:v` instead operates on all non-matching lines. You can nest `:v` into `:g` in order to [only operate on lines that match `foo`, but do not match `bar`](https://vimhelp.org/repeat.txt.html#multi-repeat "'multi-repeat'
	on vimhelp.org"). We can leverage this feature to only target actual enum fields and skip all code comments by first matching all lines that end with a comma (`,$`), and then excluding all lines from the previous match that contain two forward-slashes (`//`). We also avoided escaping the forward-slashes in the pattern with clunky syntax (`\/\/`) by just [using a different character as pattern-delimiter](https://vimhelp.org/change.txt.html#pattern-delimiter "'pattern-delimiter' on vimhelp.org").
- `norm $i = 0` is a [`normal`](https://vimhelp.org/various.txt.html#%3Anormal "':normal' on vimhelp.org") command that jumps to the end of the current line and inserts `= 0` right before your cursor.

### 1.2. Increment all numeric values sequentially

```diff
vi}o2/0,$<Enter>g<C-a>
```
- `vi}` selects all text within curly brackets.
- In visual mode, `o` [places your cursor at the other end of the current selection block](https://vimhelp.org/visual.txt.html#visual-change "'visual-change' on vimhelp.org"). Your cursor should now rest on the top-left corner of your visual block.
- `2/0,$<Enter>` [searches and jumps to the second occurrence](https://vimhelp.org/pattern.txt.html#%2F "'/' on vimhelp.org") of a zero followed by a comma at the end of a line. At this point, all zeroes except the first one should be selected.
- `g<C-a>` [sequentially increments](https://vimhelp.org/change.txt.html#CTRL-A "Command documentation on vimhelp.org") all numbers within the current selection.

Your enum class should now correctly show all of its explicit values like this [^1]:

```csharp
1public enum TranslationCodeId
 2{
 3    // buttons
 4    BUTTON_Accept = 0,
 5    BUTTON_Cancel = 1,
 6
 7    // labels
 8    LABEL_Actions = 2,
 9    LABEL_Column = 3,
10
11    // messages
12    MESSAGE_Confirm = 4,
13    MESSAGE_GoBack = 5,
14
15    // ...
16}
```

## 2\. Fixing verbose SQL insert scripts

All highlighted lines in the following script end with wrong characters:

```sql
1INSERT INTO AdminTranslationCodeText
 2    (AdminTranslationCodeTextId, AdminTranslationCodeId, LanguageId, Text)
 3    VALUES
 4    (NEWID(), 'BUTTON_Accept', 'it', 'Accetta'),
 5    (NEWID(), 'BUTTON_Accept', 'en', 'Accept'),
 6
 7    (NEWID(), 'BUTTON_Cancel', 'it', 'Annulla'),
 8    (NEWID(), 'BUTTON_Cancel', 'en', 'Cancel'),
 9
10    (NEWID(), 'LABEL_Actions', 'it', 'Azioni'),
11    (NEWID(), 'LABEL_Actions', 'en', 'Actions'),
12
13    (NEWID(), 'LABEL_Column', 'it', 'Colonna');
14    (NEWID(), 'LABEL_Column', 'en', 'Column'),
15
16    (NEWID(), 'MESSAGE_Confirm', 'it', 'Conferma'),
17    (NEWID(), 'MESSAGE_Confirm', 'en', 'Confirm');
18
19    (NEWID(), 'MESSAGE_GoBack', 'it', 'Torna indietro'),
20    (NEWID(), 'MESSAGE_GoBack', 'en', 'Go back'),
21GO
```

This usually happens when working on the same file in spurts, each time adding or rearranging lines and possibly forgetting to check their syntax. Whatever the reason behind the mistakes, how do we fix them? If you are in an IDE or, at the very least, in a text-editor with syntax highlighting support, the only way forward is to use your eyes to scan the whole file for red, squiggly lines and slowly fix all of them one at a time. You could be a little faster with shortcuts to jump through the current file errors’ position list, but you would still need to mentally evaluate the line context every time in order to correctly decide whether you must be replacing a semicolon with a comma, or viceversa. With Vim, you can completely offload the mental burden of context evaluation to your editor instead, and fix everything in one go:

```diff
:/VALUES$/+,/^GO$/-2s/;$/,/ | /^GO$/-s/,$/;/
```
- The command is made respectively by two [`substitute`](https://vimhelp.org/change.txt.html#%3Asubstitute "'substitute' on vimhelp.org") commands: the first one replaces all misplaced semicolons with commas and the second one replaces all misplaced commas with semicolons. We achieve this by prefixing each `:s` call with a custom [range](https://vimhelp.org/cmdline.txt.html#%5Brange%5D "'range' on vimhelp.org") in order to restrict the amount of text each command can interact with. In our case, we want to only replace semicolons from line 4 to line 19, and replace commas only on line 20: we can target these two ranges by [relatively referencing the positions of `VALUES` and `GO`](https://vimhelp.org/cmdline.txt.html#%3Arange-offset "'range-offset' on vimhelp.org") [^2].

## 3\. Extract field names from templates

Being able to automatically retrieve selected keywords from plain text can prove itself very useful in certain situations. For example, you might need to extract all field names from a Handlebars template snippet like this:

```handlebars
1{{#if UserDetails.FirstName}}Nome: {{UserDetails.FirstName}}{{/if}}
 2{{#if UserDetails.LastName}}Cognome: {{UserDetails.LastName}}{{/if}}
 3{{#if CallbackDetails.CallbackTimeStamp}}Data e orario indicati dal cliente per la chiamata: {{dateFormat CallbackDetails.CallbackTimeStamp format="d"}}.{{dateFormat CallbackDetails.CallbackTimeStamp format="M"}}.{{dateFormat CallbackDetails.CallbackTimeStamp format="Y"}} {{dateFormat CallbackDetails.CallbackTimeStamp format="H"}}:{{dateFormat CallbackDetails.CallbackTimeStamp format="m"}}{{/if}}
 4{{#if UserMotivation.ReasonSell}}Motivo della vendita: {{UserMotivation.ReasonSell}}{{/if}}
 5
 6Informazioni sull'immobile:
 7
 8{{#if PropertyDetails.ObjectType}}Categoria: {{PropertyDetails.ObjectType}}{{/if}}
 9{{#if PropertyDetails.ObjectSubType}}Tipo di oggetto: {{PropertyDetails.ObjectSubType}}{{/if}}
10{{#if PropertyDetails.Street}}Via: {{PropertyDetails.Street}} {{#if PropertyDetails.Number}}{{PropertyDetails.Number}}{{/if}}{{/if}}
11{{#if PropertyDetails.Zipcode}}Luogo: {{PropertyDetails.Zipcode}}{{/if}} {{#if PropertyDetails.City}}{{PropertyDetails.City}}{{/if}}
12
13Valutazione immobiliare: 
14
15{{#if ValuationDetails.EstimatedMarketValue}}Prezzo di mercato stimato: {{ValuationDetails.EstimatedMarketValue}}{{/if}}
16{{#if ValuationDetails.MinimumPrice}}Prezzo minimo:  {{ValuationDetails.MinimumPrice}}{{/if}}
17{{#if ValuationDetails.MaximumPrice}}Prezzo massimo:  {{ValuationDetails.MaximumPrice}}{{/if}}
```

You might also want to format them in a certain fashion in order to use them correctly, for example inside a SQL insert script:

```sql
1INSERT INTO Campaign
2    (CampaignId, LanguageCode, DataFields)
3    VALUES
4    (NEWID(), 'en', '/* ...insert data fields here... */');
```

Once again, you can just let Vim do most of the work for you:

Vim allows you to store arbitrary text into many different [registers](https://vimhelp.org/change.txt.html#registers "'registers' on vimhelp.org"). In order to populate one of them with all field names, we could use a nice trick for incrementally appending search results into a named register with the `substitute` command:

```diff
qhq:%s/if \(.\{-}\)\./\=setreg('H', submatch(1) . "\n")/n
```
- `qhq` clears the `h` register [^3]. This works because we are literally recording an empty [sequence of actions](https://vimhelp.org/repeat.txt.html#complex-repeat "'complex-repeat' on vimhelp.org") into it [^4], resulting in an empty string—and it is both easier and faster than typing `:let @h=''`.
- the following `substitute` command operates on all lines, but instead of replacing the matched patterns with other strings, we just save them into the `h` register using a [sub-replace expression](https://vimhelp.org/change.txt.html#sub-replace-expression "'sub-replace-expression' on vimhelp.org"). We use an uppercase `H` because that is how you [add to a register without overwriting its contents](https://vimhelp.org/change.txt.html#quote "'quote' on vimhelp.org"). The [`n` flag](https://vimhelp.org/change.txt.html#%3As_n "':s_n' on vimhelp.org") at the end tells the command not to actually substitute anything, and just evaluate any side-effects instead.

### 3.2. Remove duplicate lines and format the result

At this point, you should have all field names grouped under your `h` key. Paste its contents (`"hp`). You will see plenty of duplicate lines though, because our template had multiple instances of many different fields in the first place. You can easily remove all of them with the [`sort`](https://vimhelp.org/change.txt.html#%3Asort "':sort' on vimhelp.org") command:

```diff
\`[v\`]:'<,'>sort u
```
- `` `[v`] `` selects all pasted text. `` `[ `` lets you jump to the start of the text you just changed, while `` `] `` moves to its end.
- `:'<,'>` is a range restricting the following command only to currently-selected lines. When entering a command from Visual mode you do not have to actually type the `'<,'>` part—Vim fills it in for you.
- the `u` flag when `sort` ing only lets you keep the first of a sequence of identical lines, effectively removing any unwanted duplicate.

You can now transform the result into a nice comma-separated string:

```diff
gv:'<,'>j | s/ /,/g
```
- `gv` [re-selects your last selection](https://vimhelp.org/visual.txt.html#gv "'gv' on vimhelp.org").
- `'<,'>j` [collapses all selected lines into one line](https://vimhelp.org/change.txt.html#%3Ajoin "'join' on vimhelp.org").
- `s/ /,/g` replaces all spaces with commas. The `g` flag [replaces all occurrences in the line](https://vimhelp.org/change.txt.html#%3As_g "':s_g' on vimhelp.org"), instead of just the first one.

## 4\. Batch process multiple files

This skill can come in handy, for example, whenever you are dealing with repositories requiring small configuration changes for local development. The price you pay for not being able to`.gitignore` those changed files, though, is having all of your `git status` outputs littered by a bunch of filenames you need to remember NOT to ever commit or push to remote:

```diff
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
   modified:   ExcelGenerator/App.Debug.config
   modified:   ExcelGenerator/App.config
   modified:   Core/ConfigBase.cs
   modified:   Infrastructure/Security/Password.cs
   modified:   Tools.AgentExport/App.Debug.config
   modified:   Tools.AgentExport/App.config
   modified:   Tools.CheckLeadsForConfirmedEmail/App.Debug.config
   modified:   Tools.CheckLeadsForConfirmedEmail/App.config
   modified:   Tools.EmailSender/App.Debug.config
   modified:   Tools.EmailSender/App.config
   modified:   Tools.LeadChecker/App.Debug.config
   modified:   Tools.LeadChecker/App.config
   modified:   Tools.LeadImport/App.Debug.config
   modified:   Tools.LeadImport/App.config
   modified:   Tools.LeadManager/App.Debug.config
   modified:   Tools.LeadManager/App.config
   modified:   Tools.LeadReminder/App.Debug.config
   modified:   Tools.LeadReminder/App.config
   modified:   Tools.SmsSender/App.Debug.config
   modified:   Tools.SmsSender/App.config
   modified:   Web.Backend/Web.Backend.csproj
   modified:   Web.Backend/Web.Debug.config
   modified:   Web.Backend/Web.config

no changes added to commit (use "git add" and/or "git commit -a")
```

How can we clean this up? `git` allows you to hide changes for a given file with the `git update-index --skip-worktree {path}` command, but typing all of this out once for every file is way too painful to even consider. The answer is—you guessed it—just do it inside Vim.

### 4.1. in your current file

### 4.2. Remove all lines not containing a filename

```diff
:v/modified/d
```

### 4.3. Map all lines to git commands

```diff
gg0<C-v>tEGcgit update-index --skip-worktree <Esc>
```
- `gg0` navigates to the very start of the file.
- `<C-v>` starts a [visual block](https://vimhelp.org/visual.txt.html#visual-block "'visual-block' on vimhelp.org"); this allows you to select portions of text in a rectangular fashion. The following navigation commands (`tEG`) shape the rectangle selection so that it includes all text except the filenames themselves.
- `c` deletes the selected text and starts Insert mode. Since the selection came from a visual block, the inserted text will be simultaneously applied to all lines.

### 4.4. Execute all lines as bash commands

```diff
:w !bash
```
- `:w !{command}` [executes `command` using a range of lines as its standard input](https://vimhelp.org/editing.txt.html#%3Aw_c "':w_c' on vimhelp.org"). In our case, we call `bash` since every line is a complete `git` command.

The workflow for un-hiding files at the end of your session is exactly the same, except that in step [4.1.](https://freestingo.com/en/programming/articles/esoteric-vim/#read-git-status) you will list files using `git ls-files -v . | grep ^S`, and in [4.3.](https://freestingo.com/en/programming/articles/esoteric-vim/#skip-worktree) you will replace `--skip-worktree` with `--no-skip-worktree`.

## 5\. Integrate data from external sources into your project

The nature of this use case could not be more varied. Maybe your marketing team is sending you new translations, or your product manager is sending you new email template titles. In our example, you are sent a bunch of IPs you will need to whitelist in your C# application. This is the Jira table they currently live in (actual values obscured for privacy):

| Region | Location | CIDR |
| --- | --- | --- |
| EMEA | Manchester | XXX.XXX.XXX.XXX/20 |
| APAC | Melbourne | XXX.XXX.XXX.XXX/22 |
| APAC | Melbourne | XXX.XXX.XXX.XXX/22 |
| Americas | Washington | XXX.XXX.XXX.XXX/24 |
| Americas | Washington | XXX.XXX.XXX.XXX/18 |
| Americas | Washington | XXX.XXX.XXX.XXX/20 |
| Americas | Washington | XXX.XXX.XXX.XXX/23 |
| … | … | … |

And this is how the existing whitelisted IP list looks like:

```csharp
1var list = new List<RestrictedIPAddress>()
2{
3   new RestrictedIPAddress { Address = IPAddress.Parse("XXX.XXX.XXX.XXX"), SubnetMask = IPAddress.Parse("255.255.255.0") },
4   new RestrictedIPAddress { Address = IPAddress.Parse("XXX.XXX.XXX.XXX"), SubnetMask = IPAddress.Parse("255.255.192.0") },
5   new RestrictedIPAddress { Address = IPAddress.Parse("XXX.XXX.XXX.XXX"), SubnetMask = IPAddress.Parse("255.255.192.0") },
6   new RestrictedIPAddress { Address = IPAddress.Parse("XXX.XXX.XXX.XXX"), SubnetMask = IPAddress.Parse("255.255.192.0") },
7   // ...
8};
```

From a programmer’s perspective, the task is pretty simple: all you need to do is basically map each CIDR address into its corresponding C# object. Nonetheless, the amount of mind-numbingly boring copy-paste actions needed is directly proportional to the amount of IPs specified in the table—usually, a lot. The only excitement you will ever get out of this is finding the correct `SubnetMask` value for each subnet prefix length. Wouldn’t it be great if there was a way to let your editor instantly do all the mapping instead? This way you would not even need to double check your results since any error-prone human intervention would be removed from the actual editing. In Vim you can, with some preparations beforehand:

### 5.1. Create a Vimscript dictionary

Use subnet prefixes as keys, and `SubnetMask` addresses as values. This will come in handy later when constructing our mapping command. Copy all cells from the first table in [this cheat-sheet page](https://www.aelius.com/njh/subnet_sheet.html "Subnet Mask Cheat Sheet"), and paste them into your file. It should look something like this:

```diff
/30 4 2 255.255.255.252 1/64
/29 8 6 255.255.255.248 1/32
/28 16 14 255.255.255.240 1/16
/27 32 30 255.255.255.224 1/8
/26 64 62 255.255.255.192 1/4
/25 128 126 255.255.255.128 1/2
/24 256 254 255.255.255.0 1
/23 512 510 255.255.254.0 2
/22 1024 1022 255.255.252.0 4
/21 2048 2046 255.255.248.0 8
/20 4096 4094 255.255.240.0 16
/19 8192 8190 255.255.224.0 32
/18 16384 16382 255.255.192.0 64
/17 32768 32766 255.255.128.0 128
/16 65536 65534 255.255.0.0 256
```

Discard everything besides the first and the fourth column, and format the result into a Vimscript dictionary:

```diff
\`[v\`]:'<,'>s/^.\(\d*\).\{-}\(255.*\)\t.*/'\1': '\2',/ | '<,'>j | s/.*/{ & }/
```
- `'<,'>s/^.\(\d*\).\{-}\(255.*\)\t.*/'\1': '\2',` looks daunting but it is basically just a regex substitution in which we retain the first and fourth columns in capture groups, so that we can then format them to our liking.
- `s/.*/{ & }/` surrounds the current line in curly brackets.

Save your dictionary for later use and remove it from your file:

```diff
"mdd
```
- `"m` saves whatever text is deleted next into the `m` register.
- `dd` deletes the current line.

You can store parts of C# code that do not change between different `RestrictedIPAddress` declarations each in its own separate register to be able to not only reduce future typing errors, but also retrieve them faster.

```diff
/new Re<Enter>"ayf";;"byf";;"cy$
```
- `/new Re<Enter>` moves your cursor to a line in which a `new RestrictedIPAddress` is being instantiated.
- `"ayf"` copies `new RestrictedIPAddress { Address = IPAddress.Parse("` into register `a`.
- `;;` navigates to the next closing quote.
- `"byf"` copies `"), SubnetMask = IPAddress.Parse("` into register `b`.
- `"cy$` copies `") },` into register `c`.

### 5.3. Map CIDR addresses into new RestrictedIPAddress‘es

Copy all new addresses from your external source and paste them in the current file, whenever you want the new `RestrictedIPAddresses` to be (most likely at the end of the list). If you are lucky, your IPs will already be arranged neatly one by one on neighboring, subsequent lines. If you are unlucky and are forced to work with Jira tables, you might get something that looks more like this:

```csharp
1   new RestrictedIPAddress { Address = IPAddress.Parse("XXX.XXX.XXX.XXX"), SubnetMask = IPAddress.Parse("255.255.192.0") },
 2   new RestrictedIPAddress { Address = IPAddress.Parse("XXX.XXX.XXX.XXX"), SubnetMask = IPAddress.Parse("255.255.192.0") },
 3XXX.XXX.XXX.XXX/20
 4
 5XXX.XXX.XXX.XXX/22
 6
 7XXX.XXX.XXX.XXX/22
 8
 9XXX.XXX.XXX.XXX/24
10
11XXX.XXX.XXX.XXX/18
12
13XXX.XXX.XXX.XXX/20
14
15XXX.XXX.XXX.XXX/23
16};
```

Remove all redundant empty lines from your pasted text before continuing:

```diff
\`[v\`]:'<,'>g/^$/d
```

All the prep work is now done. Map all of your IPs into C# objects!

```diff
gv:'<,'>s/\(.*\)\/\(.*\)$/\='<C-r>a' . submatch(1) . '<C-r>b' . <C-r>m[submatch(2)] .  '<C-r>c'
```
- At its heart, this command is basically just a regex substitution in which we discard the forward-slash in the CIDR line, capture the IP address at its left, capture the subnet prefix at its right, and insert those two values in a C# line template. This would result in such a line:
	```csharp
	1   new RestrictedIPAddress { Address = IPAddress.Parse("XXX.XXX.XXX.XXX"), SubnetMask = IPAddress.Parse("20") },
	```
	The problem with that line is that the `SubnetMask` value is wrong. We need to somehow be able to tell Vim to dynamically replace those prefix lengths with actual addresses. This is exactly the purpose of sub-replace expressions. We therefore paste (`<C-r>m`) the dictionary we saved earlier on register `m` and pass it the captured prefix length (`submatch(2)`) as a key to retrieve the correct `SubnetMask` for each line.

It is worth noting that you only ever need to do the above prep work once, making the whole workflow particularly efficient for periodical tasks. Subnet mask cheat-sheets are not going to change in the foreseeable future—once you generate your Vimscript dictionary and type out [the last mapping command](https://freestingo.com/en/programming/articles/esoteric-vim/#mapping-command), you can just assign it directly to a keymap of your choice, for example `<leader>cidr` [^5]. Given you have also assigned the [empty-line remover](https://freestingo.com/en/programming/articles/esoteric-vim/#empty-line-remover) command to the `<leader>rel` keymap, for example, all you ever need to do next time you are sent a bunch of IPs to whitelist is:

```diff
p<leader>rel<leader>cidr
```

Enjoy your free time.

I had the pleasure of reading [Dimitri Sabadie’s own response to this article](https://strongly-typed-thoughts.net/blog/vim-kakoune-puzzles-2025 "'Vim vs. Kakoune puzzles' on strongly-typed-thoughts.net"). First of all, I am very glad that my article made another person out there write about something they really like and think is underrated. I currently run a heavily-customized [xmonad](https://xmonad.org/ "Xmonad home page") build on all of my Linux machines, so I know what it feels like:) I also found all of his examples to be incredibly easy to follow, even though I had never even heard of Kakoune before. Alas, while this both proves that the author did a great job explaining them and that Kakoune is indeed a powerful editor, I am not sure this also proves that Vim is “outdated”. I want to take this chance to both present my own (counter-)counterarguments to his post, and shed some more light on the unspoken parts from this article as well.

Their article immediately starts off with:

> \[“Esoteric Vim”\] dates from February 2025, so it’s a recent article about modern Vim.

This is exactly why I do not show the publication date on article pages—it makes drawing wrong assumptions too easy. I would never say any of the above tricks is “modern Vim”. If anything, they are “very-old Vim”, most of them basically being just Vi commands. Neovim is approx. two decades younger than Vim and more similar to what I would personally consider a “modern Vim” experience. I think Neovim would have made not only for a fairer, but also more interesting comparison: “the Vim approach vs. the Kakoune approach”, instead of just “new software vs. old software”. This strange assumption made his post feel more like a Kakoune advertisement to me than an actual, unbiased comparison (I suspect the author purposefully did not mention Neovim because he is aware it would have made his bias even more noticeable).

His main argument against Vim is that its regex-heavy `ex` commands are too cumbersome and difficult to read, and make for a poor interactive experience. The fact that they are presented as “the Vim way” to solve things is misleading: Vim is fundamentally an interactive editor. `ex` commands just happen to be the way I generally prefer to do things because I find them easier to reason about, in turn because I am very comfortable with regex. A complete Vim novice could easily replicate all of the above examples by just sequencing simple actions and motions into a macro, and I would still consider it an idiomatic approach. A Vim expert could probably write a Vimscript one-liner function to do what I did in three different commands, and I would still consider it an idiomatic (maybe even more so) approach [^6]. This page is not meant to show you the best or most efficient way to edit code. It is just meant to show you some useful, esoteric features and regex inevitably plays a big role in them. I am also not sure whether the sheer unreadability of regex syntax actually hinders Vim’s user-friendliness. It is true that it is “barely impossible to tell” what [this command](https://freestingo.com/en/programming/articles/esoteric-vim/#fix-sql) does by quickly glancing at it, but does it really matter? Usually `ex` commands are just something you write and execute once, and then forget about shortly after. You do not really get that many chances of reading them, and regex is far easier to write than to read. If you are aware of ranges and range offsets, writing a command that “substitutes all ending colons with commas in the space between the line after `VALUES` and two lines before `GO` ” is trivial, even though the end result looks like unintellegible clutter.

It is also arguable whether the missing interactivity is actually a bad thing. Sabadie thinks it is. Personally, I beg to disagree. The whole point of some of my commands was precisely to abstract away the need of looking at and mentally evaluating my current file contents. [This command](https://freestingo.com/en/programming/articles/esoteric-vim/#fix-sql) for example works on all (similarly-formatted).sql files that insert rows into tables, regardless of their contents—you just run it and that’s it. I prefer it not to be interactive, because instead you would have to scan your file, find out what pattern correctly targets all insert rows (in this case `NEW`, but it could be anything) and then start building your logic from there. [This other command](https://freestingo.com/en/programming/articles/esoteric-vim/#assign-zero), in a similar way, works just fine for (almost) all C# enum classes: you can just execute it and you are done. In an interactive process, you would have to select all lines that end with commas and then also check whether you accidentally selected any comment line that also ends with a comma (the Kakoune demonstration skips this part, but I assume you could achieve the same result by just filtering the selected lines). This need for redundant context awareness is exactly what I would like to free my mind of whenever possible, and would like to delegate to machines instead. It is also worth noting that you can still check your regex matches and view your changes interactively while you are writing your `ex` commands; if I am not mistaken, you can activate this feature from the builtin configuration settings in Neovim, and with a dedicated plugin in Vim.

Finally, I felt that some of the jabs were just unfair. Most of his Kakoune examples almost exactly mirror the logic of my own Vim commands, the only advantage being slightly terser syntax. About [field names](https://strongly-typed-thoughts.net/blog/vim-kakoune-puzzles-2025#3-extract-field-names-from-templates "'Vim vs.  Kakoune puzzles - Extract field names from templates' on strongly-typed-thoughts.net"), Sabadie writes that my way of extracting them is very convoluted and “absolutely crazy” because you need to copy the names to a register, remove duplicate lines and then format the result. His way, he assures, is much more simple: you just need to… copy the names to a register, remove duplicate lines and then format the result. My way of solving the `git` section problem, at a high-level, is to read the output of `git status` and map each file path into a valid `git` command. His way is to read the output of `git status` and map it into a single, valid `git` command. I think that having a single command is better than one command for each file path, but I am not sure if I would consider this a huge improvement. This is a comparison of what you would have to type for both approaches:

```diff
// vim
:v/mod/d<ret>gg0<C-v>tEGcgit update-index --skip-worktree <Esc>:w !bash<ret>

// kakoune
%smod<ret>wwlGl<a-|>xargs git update-index --skip-worktree <ret>
```

Let’s compare the enum example as well. Note that the result of the Kakoune approach is not exactly the same as the one from this page, since the values will start at `1` instead of `0`. This is what the keystrokes look like in both editors:

```diff
// vim
:let n=0 | g/,$/execute "norm $i = " . n | let n=n+1<ret>

// kakoune
%s,$<ret>i = <c-r>#
```

Even in this case the logic is virtually the same—the only difference is that in Vim you have to explicitly increase the counter variable (as far as I know). I am guessing that in order to make the index variable start at `0` instead of `1` in Kakoune, you would also have to explicitly specify it somehow. Even if it would result in just a single additional keystroke, this personally does not strike me as being “light years” ahead in terms of user interaction. It certainly does not make Vim obsolete, given the fact that this kind of processing is still unthinkable in many of the most common text editors and IDEs.

Wrapping up, though, I still think that Kakoune is intriguing. I intend to give it a chance as soon as I can, and see what comes out of it. I want to thank Sabadie again for making me discover something that I probably would have never stumbled upon on my own. This is, ultimately, what the Internet is meant for.

---

[^1]: It is also possible to do the whole thing on one pass:

This version of the command basically replaces the hardcoded `0` with an appended custom variable that is incremented on every line.

[^2]: For small scripts like the one in the example, it would actually be both easier and faster to just use line numbers:

Most times though you will be dealing with very long files spanning multiple screenfuls—retrieving the exact line numbers in those cases could be too cumbersome. Just keep in mind both approaches are nonetheless correct and will result in the same outcome.

[^3]: There is no reason we have to use that register specifically. I just find its name easier to remember (**H** andlebar templates, **m** apping stuff, etc.), but it is also possible to achieve the same results using pretty much any other named register instead.

[^4]: Somewhat counter-intuitively, just as it is possible to overwrite a register by recording a macro into it, it is also possible to [execute any string as if it were an actual sequence of actions if you save it to a register first](https://vimhelp.org/repeat.txt.html#%40 "'@' on vimhelp.org"). Try it!

[^5]: Vim stores your last executed command on the `:` read-only register.

[^6]: [Hillel Wayne](https://www.hillelwayne.com/ "Hillel Wayne") accidentally fulfilled my prediction with a great comment on [this lobste.rs thread](https://lobste.rs/s/eyceuf/vim_vs_kakoune_puzzles "'Vim vs. Kakoune puzzles' on lobste.rs"). You can just [paste, deduplicate and format your register contents](https://freestingo.com/en/programming/articles/esoteric-vim/#32-remove-duplicate-lines-and-format-the-result) with a single, elegant command: