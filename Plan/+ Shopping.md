up:: [[+ Main]]
tags:: #shopping
# Shopping
```dataview
TASK
FROM -"Work" AND -#done
WHERE !completed AND contains(tags, "#shopping")
GROUP BY file.link
```