up:: [[+ Main]]
tags::
# To do
```dataview
TASK
FROM -"Work" AND -#done
WHERE !completed AND !contains(tags, "#shopping") AND !contains(tags, "#tostudy")
GROUP BY file.link
SORT rows.file.ctime DESC
```

# Done
```dataview
TASK
FROM -"Work"
WHERE completed AND !contains(tags, "#shopping")
SORT rows.file.ctime DESC
```
