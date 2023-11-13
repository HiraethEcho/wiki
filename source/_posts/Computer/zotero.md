---
title: zotero
toc: true
date: 2023-11-11 15:09:10
tags:
categories:
published: false
---


### better note 

#### templates
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
/
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
