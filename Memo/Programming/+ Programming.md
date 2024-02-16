up:: [[+ Memo]]
tags:: #Coding  
# Programming

```dataview
TABLE
FROM "Memo/Programming"
WHERE file.path != this.file.path
SORT file.mtime desc
```