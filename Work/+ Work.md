up:: [[+ Main]]
tags:: #Work

# Work
> [!info]
> Please write notes related to work and tasks at the workplace here.
```dataview
TABLE tags
FROM #Work AND -#Work/DailyNote AND -#done
WHERE file.path != this.file.path
SORT file.mtime desc
```
## Daily Notes
```dataview
TABLE
FROM #Work/DailyNote AND -"ETC/Templates"
WHERE file.path != this.file.path
SORT file.ctime desc
LIMIT 5
```

## Working Tasks
```dataview
TASK
FROM "Work"
WHERE !completed
GROUP BY file.link
SORT rows.file.ctime DESC
```

## Done Tasks
```dataview
TASK
FROM "Work"
WHERE completed
GROUP BY file.link
SORT rows.file.ctime DESC
```