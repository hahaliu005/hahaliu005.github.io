---
title: Node js compile files
tags:
  - Nodejs
date: 2019-10-17
---

Node js compile files

<!-- more -->

```javascript
/**
 * The function for compile
 * @param {*} fromDir 
 * @param {*} toDir 
 * @param {*} compileFiles 
 */
function getCompileFiles(fromDir, toDir, compileFiles = []) {
  const files = fs.readdirSync(fromDir);
  for (let i = 0; i < files.length; i++) {
    let fromPath = fromDir + files[i];
    if (fs.lstatSync(fromPath).isFile()) {
      let toPath = toDir + files[i];
      compileFiles.push({
        from: fromPath,
        to: toPath
      })
    } else {
      compileFiles = compileFiles.concat(getCompileFiles(fromDir + files[i] + '/', toDir + files[i] + '/'))
    }
  }
  return compileFiles;
}
```