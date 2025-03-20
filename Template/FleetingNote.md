<%*
const fileName = tp.date.now("YYYY-MM-DD_HH-mm-ss");
const targetDir = "Zettelkasten/FleetingNote";
const targetPath = `${targetDir}/${fileName}`;
await tp.file.move(targetPath);
_%>
---
createdAt: <% tp.date.now("YYYY-MM-DD_THH:mm:ss") %>
literatureNote:
fleetingNote:
tags:
---