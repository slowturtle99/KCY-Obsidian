up:: [[+ Main]] 
tags:: 
# Daily Notes

```dataview
TABLE
FROM #DailyNote AND -"ETC"
WHERE file.path != this.file.path
SORT file.name desc
```

