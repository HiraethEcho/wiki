---
title: zotero
toc: true
date: 2023-11-13 18:33
tags: [wiki, obsidian, zotero]
categories:
published: true
---
# Zotero

## Install

## Plugins
**zotero 7:**
- zotmoov
- zotero better notes
 
Both:

## Sync


zotero 7 to rename:
```
{{ title case="snake" }}{{ authors case="snake" prefix="_" max="1"}}{{ year prefix="_" }}
```
using zotmoov to move files

## zotero better notes

**templates:**
```yaml
-
    name: '[QuickInsertV2]'
    text: "// @use-markdown\n${linkText} [ob](/zotero/${subNoteItem.getNoteTitle ? subNoteItem.getNoteTitle().replace(/[/\\\\?%*:|\"<> ]/g, \"-\") + \"-\":\"\"}${subNoteItem.key}) <a href=\"${link}\">zn</a>"
-
    name: '[QuickImportV2]'
    text: "<blockquote>\n${{\n  return await Zotero.BetterNotes.api.convert.link2html(link, {noteItem, dryRun: _env.dryRun});\n}}$\n</blockquote>"
-
    name: '[QuickNoteV5]'
    text: "${annotationItem.annotationComment}\n${{\n  let res = \"\";\n  res += await Zotero.BetterNotes.api.convert.annotations2html([annotationItem], {noteItem, ignoreComment: true});\n  return res;\n}}$"
-
    name: '[ExportMDFileNameV2]'
    text: '${noteItem.parentItem ? "A-":""}${(noteItem.getNoteTitle ? noteItem.getNoteTitle().replace(/[/\\?%*:|"<> ]/g, "-") + "-" : "")}${noteItem.key}.md'
-
    name: '[ExportMDFileHeaderV2]'
    text: "${{\n  let header = {};\n  header.tags = noteItem.getTags().map((_t) => _t.tag);\n  header.parent = noteItem.parentItem\n    ? noteItem.parentItem.getField(\"title\")\n    : \"\";\n  return JSON.stringify(header);\n}}$"
-
    name: '[ExportMDFileContent]'
    text: "${{\n\tlet str = mdContent;\n\tlet rmspan = str.replace(/<\\/?span.*?>/g, '');\n\tlet isolink = rmspan.replace(/(<a.*?>)“(.*?)”<\\/a>/g,'$2 $1(to zotero pdf)</a>');\n\treturn isolink;\n}}$"
```
