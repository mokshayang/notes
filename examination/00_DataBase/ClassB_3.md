---
author: moksha
rating: "10"
tags:
  - exam
  - mysql
---
## 第三題的資料庫設定  3個資料表

> [!note] 院線片   ***$Movie => movie***     &nbsp; &nbsp; &nbsp; &nbsp;  總項目->12 , 初始化 ->10

|   name   | type  | deffault value | attribute | remark   |
|:--------:|:-----:|:--------------:|:---------:| -------- |
|    id    | int10 |                | unsigned  | id       |
|   name   |   T   |                |  -------  | 電影名稱 |
|  level   | int1  |                | unsigned  | 分級     |
|  length  | int3  |                | unsigned  | 片長     |
|  intro   |   T   |                |  -------  | 簡介     |
|   date   | date  |                |  -------  | 上映日期 |
| publish  |   T   |                |  -------  | 發行商   |
| director |   T   |                |  -------  | 導演     |
| trailer  |   T   |                |  -------  | 預告片   |
|  poster  |   T   |                |  -------  | 海報     |
|   rank   | int1  |                | unsigned  | 順序     |
|    sh    | int1  |       1        | unsigned  | 是否顯示 |

---

> [!warning] 電影海報   ***$Sp => pos***  &nbsp; &nbsp; &nbsp; &nbsp;  總項目->6 , 初始化 ->4

| name | type  | deffault value | attribute | remark   |
|:----:|:-----:|:--------------:|:---------:| -------- |
|  id  | int10 |                | unsigned  | id       |
| name |   T   |                |  -------  | 電影名稱 |
| img  |   T   |                |  -------  | 電影海報  |
| ani  | int1  |                | unsigned  | 動畫代碼 |
| rank | int1  |                | unsigned  | 順序     |
|  sh  | int1  |       1        | unsigned  | 是否顯示 |

---

> [!important] 訂單 ***$Ord => ord***  &nbsp; &nbsp; &nbsp; &nbsp; 總項目->7 , 初始化 ->6

|  name   | type  | deffault <br> value | attribute | remark               |
|:-------:|:-----:|:-------------------:|:---------:| -------------------- |
|   id    | int10 |                     | unsigned  | id                   |
|   sn    |   T   |                     |  -------  | 序號 *serial number* <br> 日期+4個號碼|
|  name   |   T   |                     |  -------  | 電影名稱             |
|  date   | date  |                     | unsigned  | 日期                 |
| session |   T   |                     |  -------  | 時間                 |
|  seats  |   T   |                     | unsigned  | 座位訊息 (序列化)    | 
|   qt    | int1  |                     |           | 票數                 |

---

> [!warning] &nbsp;  &nbsp;  &nbsp;  &nbsp;  初始化

```php
<?php
include_once "api/base.php";
$Movie->ddd();
$Sp->ddd();
$Ord->ddd();

for($i=1;$i<10;$i++){
    $data = [];
    $data['name'] = "預告片$i";
    $data['img'] = "03a0$i.jpg";
    $data['rank'] = $i;
    $data['ani'] = rand(1,3);

    $Pos->save($data);
}
$date = [
    date("Y-m-d",strtotime("-2 day")),
    date("Y-m-d",strtotime("-1 day")),
    date("Y-m-d"),
    date("Y-m-d",strtotime("+1 day")),
    date("Y-m-d",strtotime("+2 day"))
];

for($i=1;$i<26;$i++){
    $data = [];
    $data['name'] = "院線片$i";
    $data['level'] = rand(1,4);
    $data['length'] = rand(90,120);
    $data['date'] = $date[rand(0,4)];
    $data['intro'] = "劇情介紹$i";
    $data['publish'] = "發行商";
    $data['director'] = "導演";
    $data['trailer'] = "03b".sprintf("%02d",$i)."v.mp4";
    $data['poster'] = "03b".sprintf("%02d",$i).".png";
    $data['rank'] = $i;
    
    $Movie->save($data);
}

for($i=1;$i<61;$i++){
    $data = [];
    $data['sn'] = date("Ymd").sprintf("%04d",$i);
    $data['name'] = "院線片".rand(1,25);
    $data['date'] = $date[rand(0,4)];
    $data['session'] = $Ord->at[rand(6,10)];
    $data['qt'] = rand(1,4);

    for($j=0;$j<$data['qt'];$j++){
        $data['seats'][] = rand(0,19);
    }
    $data['seats'] = serialize($data['seats']);
    
    $Ord->save($data);
}
?>
<a href="index.php"><button>首頁</button></a>

```

## 關於初始化  :

```php
class DB
{
	.....
    function ddd(){
        $sql = "truncate $this->table ";
        return $this->pdo->exec($sql);
    }
    .....
}

$Sp = new DB("poster");

```
