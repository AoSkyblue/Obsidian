<%*
const fileName = tp.date.now("YYYY-MM-DD-HH-mm-ss");
const targetDir = "Zettelkasten/PermanentNote";
const targetPath = `${targetDir}/${fileName}`;
await tp.file.move(targetPath);
_%>
/
