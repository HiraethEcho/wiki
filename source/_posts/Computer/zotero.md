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

## Sync
zotero 7 to rename:
```
{{ title case="snake" }}{{ authors case="snake" prefix="_" max="1"}}{{ year prefix="_" }}
```

## Plugins
For zotero 6:

For zotero 7:
- zotmoov
### better note 

#### templates
```yaml
-
    name: '[QuickInsertV2]'
    text: "<p>\n    ${linkText}</br>\n\t${subNoteItem.getNote().trim() }\n    [obidian](/wiki/zotero/${(noteItem.getNoteTitle ? noteItem.getNoteTitle().replace(/[/\\\\?%*:|\"<> ]/g, \"-\") + \"-\" : \"\")}${noteItem.parentItem ? noteItem.parentItem.getField(\"title\").replace(/ /g, \"\") + \"-\" : \"\"}${noteItem.key})<br>\n    <a href=\"${link}\">\n      zotero\n    </a>\n</p>"
-
    name: '[QuickBackLinkV2]'
    text: "<p>\n    Referred in ${linkText} <a href=\"${link}\">zotero</a> [obidian](/wiki/zotero/${(noteItem.getNoteTitle ? noteItem.getNoteTitle().replace(/[/\\\\?%*:|\"<> ]/g, \"-\") + \"-\" : \"\")}${noteItem.parentItem ? noteItem.parentItem.getField(\"title\").replace(/ /g, \"\") + \"-\" : \"\"}${noteItem.key})\n</p>"
-
    name: '[QuickImportV2]'
    text: "<p>\n  ${await new Promise(async (r) => {\n    r(await Zotero.BetterNotes.api.convert.link2html(link,{noteItem, dryRun: _env.dryRun}));\n  })}\n</p>"
-
    name: '[QuickNoteV5]'
    text: "${await new Promise(async (r) => {\n    let res = \"\";\n    if (annotationItem.annotationComment) {\n      res += await Zotero.BetterNotes.api.convert.md2html(\n        annotationItem.annotationComment\n      );\n    }\n    res += await Zotero.BetterNotes.api.convert.annotations2html([annotationItem], {noteItem, ignoreComment: true});\n    r(res);\n  })}\n"
-
    name: '[ExportMDFileNameV2]'
    text: '${(noteItem.getNoteTitle ? noteItem.getNoteTitle().replace(/[/\\?%*:|"<> ]/g, "-") + "-" : "")}${noteItem.parentItem ? noteItem.parentItem.getField("title").replace(/ /g, "") + "-" : ""}${noteItem.key}.md'
-
    name: '[ExportMDFileHeaderV2]'
    text: "${await new Promise(async (r) => {\n    let header = {};\n    header.tags = noteItem.getTags().map((_t) => _t.tag);\n\n    header.title = noteItem.getField(\"title\") + \"-\" + ( noteItem.parentItem\n      ? noteItem.parentItem.getField(\"title\").replace(/ /g, \"\") + \"-\"\n      : \"\") + noteItem.getField(\"key\");\n\n    header.article = noteItem.parentItem\n      ? noteItem.parentItem.getField(\"title\").replace(/ /g, \"\")\n      : \"\";\n    header.collections = (\n      await Zotero.Collections.getCollectionsContainingItems([\n        (noteItem.parentItem || noteItem).id,\n      ])\n    ).map((c) => c.name);\n    r(JSON.stringify(header));\n  })}\n\n"
-
    name: '[ExportMDFileContent]'
    text: "${mdContent.replaceAll(\"\\\\[obsidian]\\\\(\",\"[obsidian](\")}\n\t"

```
Quick insert
```
<p>
    ${linkText}</br>
    [obidian](/wiki/zotero/${(noteItem.getNoteTitle ? noteItem.getNoteTitle().replace(/[/\\?%*:|"<> ]/g, "-") + "-" : "")}${noteItem.parentItem ? noteItem.parentItem.getField("title").replace(/ /g, "") + "-" : ""}${noteItem.key})<br>
    <a href="${link}">
      zotero
    </a>
</p>
```
Quick back link
```
<p>
    Referred in ${linkText}(
    <a href="${link}">
      zotero
    </a>
[obidian](/wiki/zotero/${(noteItem.getNoteTitle ? noteItem.getNoteTitle().replace(/[/\\?%*:|"<> ]/g, "-") + "-" : "")}${noteItem.parentItem ? noteItem.parentItem.getField("title").replace(/ /g, "") + "-" : ""}${noteItem.key})
)
</p>
```
Quick import
```
<p>
  ${await new Promise(async (r) => {
    r(await Zotero.BetterNotes.api.convert.link2html(link,{noteItem, dryRun: _env.dryRun}));
  })}
</p>
```
Quick Note
```
${await new Promise(async (r) => {
    let res = "";
    if (annotationItem.annotationComment) {
      res += await Zotero.BetterNotes.api.convert.md2html(
        annotationItem.annotationComment
      );
    }
    res += await Zotero.BetterNotes.api.convert.annotations2html([annotationItem], {noteItem, ignoreComment: true});
    r(res);
  })}
```

Export filename
```
${(noteItem.getNoteTitle ? noteItem.getNoteTitle().replace(/[/\\?%*:|"<> ]/g, "-") + "-" : "")}${noteItem.parentItem ? noteItem.parentItem.getField("title").replace(/ /g, "") + "-" : ""}${noteItem.key}.md
```

Export header
```
${await new Promise(async (r) => {
    let header = {};
    header.tags = noteItem.getTags().map((_t) => _t.tag);

    header.title = noteItem.getField("title") + "-" + ( noteItem.parentItem
      ? noteItem.parentItem.getField("title").replace(/ /g, "") + "-"
      : "") + noteItem.getField("key");

    header.article = noteItem.parentItem
      ? noteItem.parentItem.getField("title").replace(/ /g, "")
      : "";
    header.collections = (
      await Zotero.Collections.getCollectionsContainingItems([
        (noteItem.parentItem || noteItem).id,
      ])
    ).map((c) => c.name);
    r(JSON.stringify(header));
  })}
```
