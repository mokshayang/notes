---
author: moksha
rating: 8
tags:
  - exam
  - mysql
---
## 第二題的資料庫設定 5個資料表


> [!note] 瀏覽人數 ***$Total => total***  &nbsp; &nbsp; &nbsp; &nbsp; 總項目->3

| name  | type  | deffault<br>value | attribute | remark     |     
|:-----:|:-----:|:-----------------:|:---------:| ---------- | 
|  id   | int10 |                   | unsigned  | id         | 
| date  | date  |                   |  -------  | 日期(手動) | 
| total | int10 |                   | unsigned  | 當天人數   |    

---

> [!note] 新聞資料 ***$News => news***  &nbsp; &nbsp; &nbsp; &nbsp; 總項目->8

| name  | type  | deffault<br>value | attribute | remark   |     |
|:-----:|:-----:|:-----------------:|:---------:| -------- | --- |
|  id   | int10 |                   | unsigned  | id       |     |
| title |   T   |                   |  -------  | 新聞標題 |     |
| text  |   T   |                   |  -------  | 內容     |     |
| type  |   T   |                   |  -------  | 4個種類  |     |
| good  | int10 |                   |  -------  | 點讚數   |     |
|  sh   | int1  |                   |  -------  | 顯示與否 |     |


> [!note] 問卷調查 ***$Que => que***  &nbsp; &nbsp; &nbsp; &nbsp; 總項目->4

|  name  | type  | deffault<br>value | attribute | remark   |     
|:------:|:-----:|:-----------------:|:---------:| -------- | 
|   id   | int10 |                   | unsigned  | id       |     
|  text  |   T   |                   |  -------  | 內容     |     
| parent | int10 |                   |  -------  |          |     
| count  | int10 |                   |  -------  | 投票數   |     



> [!warning] 會員資料 ***$User => user***  &nbsp; &nbsp; &nbsp; &nbsp; 總項目->4

| name  | type  | deffault<br>value | attribute | remark   |     
|:-----:|:-----:|:-----------------:|:---------:| -------- | 
|  id   | int10 |                   | unsigned  | id       |     
|  acc  |   T   |                   |  -------  | 帳號     |     
|  pw   |   T   |                   |  -------  | 密碼     |     
| email |   T   |                   |  -------  | 電子信箱 |     



> [!warning] 會員點讚紀錄 ***$Log => log***  &nbsp; &nbsp; &nbsp; &nbsp; 總項目->3

| name | type  | deffault<br>value | attribute | remark  |     
|:----:|:-----:|:-----------------:|:---------:| ------- | 
|  id  | int10 |                   | unsigned  | id      |     
| user |   T   |                   |  -------  | user_acc <br> $_SESSION['login']|$     
| news |   T   |                   |  -------  | news_id |     

#### 關於瀏覽人數 : 

```php
//先去抓今天的日期:
$total = $Total->find(['date' => date("Y-m-d")]);

//無今天的日期時:
if (empty($total)) { //判斷有無今日日期
    $Total->save(['date' => date("Y-m-d"), "total" => 1]);//此無id 為新增data
    $total = $Total->find(['date' => date("Y-m-d")]);//此時的$total 有id
}
//針對 client 紀錄人數
if (empty($_SESSION['total'])) {
    $total['total']++;
    $Total->save($total);
    $_SESSION['total'] = $total['total'];
}
/**
今日瀏覽: $total['total'] // session 紀錄今日的瀏覽日;
累積瀏覽: $Total->sum('total');
**/
```

#### 與第一題的比較 :

```php
$num = $Total->find(1);

if(empty($_SESSION['visit'])){
    $num['num']++;
    $Total->save($num);
    $_SESSION['visit']=1;
}
```

