---
cssclasses:
  - dashboard
banner: "![[IMG_0073.jpg]]"
banner_y: 0.604
---


- ## Status
	- **Notes**:  `$=dv.pages().length`
	- **Recently Modified**
	```dataview
	LIST
	FROM "" AND -#DailyNote AND -#Work
	SORT file.mtime desc
	LIMIT 3
	```

- ## Daily Notes
	```dataview
	LIST
	FROM #DailyNote AND -"ETC"
	WHERE file.path != this.file.path
	SORT file.ctime desc
	LIMIT 5
	```
- ## Plans
	```dataview
	LIST
	FROM #Plan AND -#done AND -"Plan/+ Plans"
	SORT file.mtime desc
	```
	
- ## Cooking
	- 📜 [[+ Recipes|Recipes]]
	- 📒[[+ Tips & How To|Tips & How To]]
	```dataview
	LIST
	FROM #Cooking/Recipe
	SORT file.mtime desc
	LIMIT 3
	```
- ## Study
	- 🔬 [[+ Study|Study]]
	- **Recently Modified**
	```dataview
	LIST
	FROM "Study"
	SORT file.mtime desc
	LIMIT 3
	```
- ## To Study
	```dataview
	TASK
	FROM -"Work" AND -#done
	WHERE !completed AND contains(tags, "#tostudy")
	SORT rows.file.ctime DESC
	LIMIT 3
	```

---
## Navigation
- 📝 [[+ Daily Notes|Daily Notes]]
- 🔬 [[+ Study|Study]]
- 🏗️ [[+ Projects|Projects]]
- 📚 [[+ Memo|Memo]]
- 🔭 [[+ Plans|Plans]]
- ✅ [[+ To Do|To Do]]
- 🧾 [[+ Shopping|Shopping]]
- 💼 [[+ Work|Work]]
- 🗃️ [[+ Archive|Archive]]

---
##  To Do
```dataview
TASK
FROM -"Work" AND -#done
WHERE !completed AND !contains(tags, "#shopping") AND !contains(tags, "#tostudy")
GROUP BY file.link
SORT rows.file.ctime DESC
```