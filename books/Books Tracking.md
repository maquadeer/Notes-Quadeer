---
dg-publish: true
---
```dataview 
Table author as Author, ("![|80](" + cover + ")") as Cover, pages, category as genre
From "books" 
Where contains(status, "complete") 
```