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
    text: "<p>\n\t${subNoteItem.getNote().trim() }\n    [md](/wiki/zotero/${(subNoteItem.getNoteTitle ? subNoteItem.getNoteTitle().replace(/[/\\\\?%*:|\"<> ]/g, \"-\") + \"-\" : \"\")}${subNoteItem.parentItem ? \"Article-\" : \"Main-\"}${subNoteItem.key})\n    <a href=\"${link}\">\n      zotero\n    </a>\n</p>"
-
    name: '[QuickBackLinkV2]'
    text: "<p>\n    Referred in ${linkText}:[obsidian](/wiki/zotero/${(noteItem.getNoteTitle ? noteItem.getNoteTitle().replace(/[/\\\\?%*:|\"<> ]/g, \"-\") + \"-\" : \"\")}${noteItem.parentItem ? \"Article-\" : \"Main-\"}${noteItem.key}) <a href=\"${link}\">zotero</a>\n\t  </p>"
-
    name: '[QuickImportV2]'
    text: "<p>\n  ${await new Promise(async (r) => {\n    r(await Zotero.BetterNotes.api.convert.link2html(link,{noteItem, dryRun: _env.dryRun}));\n  })}\n</p>"
-
    name: '[QuickNoteV5]'
    text: "${await new Promise(async (r) => {\n    let res = \"\";\n    if (annotationItem.annotationComment) {\n      res += await Zotero.BetterNotes.api.convert.md2html(\n        annotationItem.annotationComment\n      );\n    }\n    res += await Zotero.BetterNotes.api.convert.annotations2html([annotationItem], {noteItem, ignoreComment: true});\n    r(res);\n  })}\n"
-
    name: '[ExportMDFileNameV2]'
    text: "${(noteItem.getNoteTitle ? noteItem.getNoteTitle().replace(/[/\\\\?%*:|\"<> ]/g, \"-\") + \"-\" : \"\")}${noteItem.parentItem\n      ? \"Article-\"\n      : \"Main-\"}${noteItem.key}.md"
-
    name: '[ExportMDFileHeaderV2]'
    text: "${await new Promise(async (r) => {\n    let header = {};\n    header.tags = noteItem.getTags().map((_t) => _t.tag);\n\n    header.title = noteItem.getField(\"title\") + \"-\" + noteItem.getField(\"key\");\n\n    header.article = noteItem.parentItem\n      ? noteItem.parentItem.getField(\"title\").replace(/ /g, \"\")\n      : \"\";\n    header.collections = (\n      await Zotero.Collections.getCollectionsContainingItems([\n        (noteItem.parentItem || noteItem).id,\n      ])\n    ).map((c) => c.name);\n    r(JSON.stringify(header));\n  })}\n\n"
-
    name: '[ExportMDFileContent]'
    text: "${mdContent.replaceAll(\"\\\\[md]\\\\(\",\"[md](\")}\n\t"
```

