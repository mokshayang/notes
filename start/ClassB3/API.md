## base

0. base.php

## JS.js
```js
function sw(table, id1, id2) {
    $.get("api/sw.php", { table, id1, id2 }, () => {
        location.reload();
    })
}
function del(table, id) {
    let chk = confirm("確定要刪除嗎")
    if (chk) {
        $.get("api/del.php", { table, id }, () => {
            location.reload();
        })
    }
}
function show(id) {
    $.get("api/show.php", { id }, () => {
        location.reload();
    })
}
```


1. del.php
```php
$db = new DB($_GET['table']);
$db->del($_GET['id']);
```
2.  sw.php
```php
$db = new DB($_GET['table']);
$t1 = $db->find($_GET['id1']);
$t2 = $db->find($_GET['id2']);
$tmp = $t1['rank'];
$t1['rank'] = $t2['rank'];
$t2['rank'] = $tmp;
$db->save($t1);
$db->save($t2);
```
3.  show.php
```php
$sh = $Movie->find($_GET['id']);
$sh['sh'] = ($sh['sh']+1)%2;
$Movie->save($sh);
```

## booking.php(選擇座位)  ord.php (付帳) 
## 對應 front/ord.php(線上訂票)

4. booking.php

5. ord.php

![[Pasted image 20230612234300.png]]
### 重點中的重點 50%

## front/ord.php 衍生的

7. movs.php (電影名稱)

8. days.php (哪天上映)

9. session.php (場次) => 14:00-16:00

## 三個後臺

10. pos_edit.php  => ( back/pos.php )
11. pos_add.php  => ( back/pos.php )

12. save.php => ( back/movie_add.php  &&  back/movie_edit.php )


### 最後一個

13.  qdel.php  => ( back/ord.php 訂單管理 )