<%*
const fileName = tp.date.now("YYYY-MM-DD");
const targetDir = "Daily";
const targetPath = `${targetDir}/${fileName}`;
await tp.file.move(targetPath);
_%>
---
createdAt: <% tp.date.now("YYYY-MM-DD_THH:mm:ss") %>
tags:
---