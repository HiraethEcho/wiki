---
title: zotero
toc: true
date: 2023-11-13 18:33
tags: [wiki, obsidian, zotero]
categories:
published: true
---
# Zotero

## Install and basic setting

## Plugins
**zotero 7:**
- zotmoov
- zotero better notes
 

## Sync
using zotmoov to move files

zotero 7 to rename:
```
{{ title case="snake" }}{{ authors case="snake" prefix="_" max="1"}}{{ year prefix="_" }}
```

use any cloud drive to sync. I'm using OneDrive.
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
Or you can copy these:
```quickinsert
# This template is specifically for importing/sharing, using better 
# notes 'import from clipboard': copy the content and
# goto Zotero menu bar, click Tools->New Template from Clipboard.  
# Do not copy-paste this to better notes template editor directly.
name: "[QuickInsertV2]"
zoteroVersion: "7.0.9.SOURCE.fadbf3d2d"
pluginVersion: "2.0.18"
savedAt: "2024-11-07T05:54:05.660Z"
content: |-
  // @use-markdown
  [${linkText}](/wiki/zotero/${subNoteItem.getNoteTitle ? subNoteItem.getNoteTitle().replace(/[/\\?%*:|"<> ]/g, "-") + "-":""}${subNoteItem.key}) <a href="${link}">zn</a>
```
```quickimport
# This template is specifically for importing/sharing, using better 
# notes 'import from clipboard': copy the content and
# goto Zotero menu bar, click Tools->New Template from Clipboard.  
# Do not copy-paste this to better notes template editor directly.
name: "[QuickImportV2]"
zoteroVersion: "7.0.9.SOURCE.fadbf3d2d"
pluginVersion: "2.0.18"
savedAt: "2024-11-07T05:54:37.696Z"
content: |-
  ${link}
  <blockquote>
  ${{
    return await Zotero.BetterNotes.api.convert.link2html(link, {noteItem, dryRun: _env.dryRun});
  }}$
  </blockquote>

```
```quicknote
# This template is specifically for importing/sharing, using better 
# notes 'import from clipboard': copy the content and
# goto Zotero menu bar, click Tools->New Template from Clipboard.  
# Do not copy-paste this to better notes template editor directly.
name: "[QuickNoteV5]"
zoteroVersion: "7.0.9.SOURCE.fadbf3d2d"
pluginVersion: "2.0.18"
savedAt: "2024-11-07T05:54:55.255Z"
content: |-
  ${{
    let res = "";
    if (annotationItem.annotationComment) {
      res += await Zotero.BetterNotes.api.convert.md2html(
        annotationItem.annotationComment
      );
    } else {
  	res += "No comment";
    }
    return res;
  }}$
  
  // @use-markdown
  ***
  ${{
  	let res = "";
    res += await Zotero.BetterNotes.api.convert.annotations2html([annotationItem], {noteItem, ignoreComment: true});
    return res;
  }}$
```

```exportmdfilename
# This template is specifically for importing/sharing, using better 
# notes 'import from clipboard': copy the content and
# goto Zotero menu bar, click Tools->New Template from Clipboard.  
# Do not copy-paste this to better notes template editor directly.
name: "[ExportMDFileNameV2]"
zoteroVersion: "7.0.9.SOURCE.fadbf3d2d"
pluginVersion: "2.0.18"
savedAt: "2024-11-07T05:55:24.890Z"
content: |-
  ${(noteItem.getNoteTitle ? noteItem.getNoteTitle().replace(/[/\\?%*:|"<> ]/g, "-") + "-" : "")}${noteItem.key}.md
```
```exportmdfileheader
# This template is specifically for importing/sharing, using better 
# notes 'import from clipboard': copy the content and
# goto Zotero menu bar, click Tools->New Template from Clipboard.  
# Do not copy-paste this to better notes template editor directly.
name: "[ExportMDFileHeaderV2]"
zoteroVersion: "7.0.9.SOURCE.fadbf3d2d"
pluginVersion: "2.0.18"
savedAt: "2024-11-07T05:55:34.047Z"
content: |-
  ${{
    let header = {};
    header.tags = noteItem.getTags().map((_t) => _t.tag);
    header.parent = noteItem.parentItem
      ? noteItem.parentItem.getField("title")
      : "";
    header.collections = (
      await Zotero.Collections.getCollectionsContainingItems([
        (noteItem.parentItem || noteItem).id,
      ])
    ).map((c) => c.name);
    return JSON.stringify(header);
  }}$
```
```exportmdfilecontent
# This template is specifically for importing/sharing, using better 
# notes 'import from clipboard': copy the content and
# goto Zotero menu bar, click Tools->New Template from Clipboard.  
# Do not copy-paste this to better notes template editor directly.
name: "[ExportMDFileContent]"
zoteroVersion: "7.0.9.SOURCE.fadbf3d2d"
pluginVersion: "2.0.18"
savedAt: "2024-11-07T05:56:22.434Z"
content: |-
  ${{
    let start = mdContent;
    let rmspan = start.replace(/<\/?span.*?>/g,'');
    let pdflink = rmspan.replace(/(<a .*?open.*?>)“(.*?)”/g,'$2 $1(pdf)</a>');
    let dir2zotero = pdflink.replace(/<a href.*?zhref="(.*?)" ztype.*?>/g,'<a href="$1">');
    return dir2zotero;
  }}$
```
