---
author: moksha
rating: 8
tags:
  - exam
  - mysql
---
## 第一題的資料庫設定 9個資料表


### ad (動態文字)
|     name      | type  | deffault<br>value | attribute | remark   |
|:-------------:|:-----:|:-----------------:|:---------:| -------- |
|      id       | int10 |                   | unsigned  | id       |
|     text      |   T   |                   |  -------  | 內容     |
|      sh       | int1  |         1         | unsigned  | 顯示與否 |

### news (最新消息)
|     name      | type  | deffault<br>value | attribute | remark   |
|:-------------:|:-----:|:-----------------:|:---------:| -------- |
|      id       | int10 |                   | unsigned  | id       |
|     text      |   T   |                   |  -------  | 內容     |
|      sh       | int1  |         1         | unsigned  | 顯示與否 |

---

### image (校園映像)
|   name   | type  | deffault<br>value | attribute | remark   |
|:--------:|:-----:|:-----------------:|:---------:| -------- |
|    id    | int10 |                   | unsigned  | id       |
|   img    |   T   |                   |  -------  | 圖片     |
|    sh    | int1  |         1         | unsigned  | 顯示與否 |

### mvim (校園動畫)
|   name   | type  | deffault<br>value | attribute | remark   |
|:--------:|:-----:|:-----------------:|:---------:| -------- |
|    id    | int10 |                   | unsigned  | id       |
|   img    |   T   |                   |  -------  | 圖片     |
|    sh    | int1  |         1         | unsigned  | 顯示與否 |

---
### title (標題網站)
| name | type  | deffault<br>value | attribute | remark   |
|:----:|:-----:|:-----------------:|:---------:| -------- |
|  id  | int10 |                   | unsigned  | id       |
| img  |   T   |                   |  -------  | 圖片     |
| text |   T   |                   |  -------  | 內容     |
|  sh  | int1  |      nothing      | unsigned  | 顯示與否 |

### admin (帳號管理)
| name | type  | deffault<br>value | attribute | remark |
|:----:|:-----:|:-----------------:|:---------:| ------ |
|  id  | int10 |                   | unsigned  | id     |
| acc  |   T   |                   |  -------  | 帳號   | 
|  pw  |   T   |                   |  -------  | 密碼   |

### menu (選單管理)
|   name    | type  | deffault<br>value | attribute | remark   |
|:---------:|:-----:|:-----------------:|:---------:| -------- |
|    id     | int10 |                   | unsigned  | id       |
| ***url*** |   T   |                   |  -------  | 圖片     |
|   text    |   T   |                   |  -------  | 內容     |
|    sh     | int1  |         1         | unsigned  | 顯示與否 |
|  parent   | int10 |                   | unsigned  | 父層     | 


---
### bottom (頁尾版權)
| name | type  | deffault<br>value | attribute | remark   |
|:----:|:-----:|:-----------------:|:---------:| -------- |
|  id  | int10 |                   | unsigned  | id       |
| text |   T   |                   |  -------  | 版權內容 | 

### total (進站人數)
| name  | type  | deffault<br>value | attribute | remark |
|:-----:|:-----:|:-----------------:|:---------:| ------ |
|  id   | int10 |                   | unsigned  | id     |
| total | int10 |                   |  -------  | 總人數 | 

---
## 關於資料表的建立

### 先建立 title ， copy 8份 ， 再去改欄位

### 除了bottom、total除外，其他七項，在back 中的 form 一律進 api  =>  edit.php

### #注意 : 

```php
// 此檔案 適用單項，也就是只有一個的項目要修改
// 用oop isset(id) 來判斷 要 edit or add
// 若需要修改一大堆 ， 則取用 api/edit.php
// 差別在於 form 表單中的 name = "xxx[]" 是否為array
// ex name = "id" value = 1
// ex name = "id[]" value =  $row['id']  這邊一般會有foreach 99%
// 所以此api 適用於單項

$db = new db($_POST['tab']);
$tab = $_POST['tab'];
dd($_POST);
unset($_POST['tab']);
if(!empty($_FILES['img']['tmp_name'])){
    move_uploaded_file($_FILES['img']['tmp_name'], "../upload/".$_FILES['img']['name']);
    $_POST['img'] = $_FILES['img']['name'];
}
$db->save($_POST);
to("../admin.php?do=$tab");
```

```php
// 修改多項時候 : 
$db = new db($_POST['tab']);
$tab = $_POST['tab'];
unset($_POST['tab']);
$data = [];

if(isset($_POST['id'])){
    foreach($_POST['id'] as $idx => $id){
        if(isset($_POST['del']) && in_array($id,$_POST['del'])){
            $db->del($id);
        }else{
            foreach($_POST as $col => $val){
                if($col == 'sh'){
                    if(is_array($val)){
                        $data['sh'] = in_array($id,$val)?1:0;
                    }else{
                        $data['sh'] = ($id == $val)?1:0;
                    }
                }else{
                    if($col != 'del') $data[$col] = $val[$idx];
                    // 這邊一定要 if($col != 'del')
                    // 因為是一起送過來的，雖然上面把有['del']的內容刪除，
                    // 但並未 unset[$_post['del']_]
                    // 所以$_post['del']的職，還是存在的
                }
            }
            $db->save($data);
        }
    }
}

to("../admin.php?do=$tab");
```

#### 瀏覽總人數:

```php
$num = $Total->find(1);
$bot= $Bottom->find(1);

if(empty($_SESSION['visit'])){
    $num['num']++;
    $Total->save($num);
    $_SESSION['visit']=1;
}

```
