```dataview
table file.name as 檔名, file.size as 大小, rating as 級分, author as 作者
from #日記
where rating > 5
sort file.name desc
```
