up:: [[+ Main]]
tags::
___
# Study

```dataview
TASK
FROM -"Work" AND -#done
WHERE !completed AND contains(tags, "#tostudy")
SORT rows.file.ctime DESC
```


