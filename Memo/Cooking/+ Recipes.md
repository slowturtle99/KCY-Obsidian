#Cooking/Recipe 

# Confirmed Recipes

```dataview
TABLE
    Cusine,
    Type
FROM "Memo/Cooking/Recipes"
WHERE
    Status = "Confirmed" 
SORT file.mtime desc
```

# Complete Recipes

```dataview
TABLE
    Cusine,
    Type
FROM "Memo/Cooking/Recipes"
WHERE
    Status = "Complete" 
SORT file.mtime desc
```

# Working Recipes

```dataview
TABLE
    Cusine,
    Type
FROM "Memo/Cooking/Recipes"
WHERE
    Status = "Working" 
SORT file.mtime desc
```