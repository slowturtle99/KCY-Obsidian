up:: [[+ Main]]
tags:: #Plan 
# Plans

```dataview
TABLE
FROM #Plan AND -#done
WHERE file.path != this.file.path
SORT file.mtime desc
```
