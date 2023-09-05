---
author: moksha
rating: 8
tags:
  - exam
  - mysql
---
**** ## 第四題的資料庫設定 7個資料表

> [!note] 管理者帳號   ***$Admin => admin***     &nbsp; &nbsp; &nbsp; &nbsp;  總項目->4 

| name | type  | deffault value | attribute | remark            |
|:----:|:-----:|:--------------:|:---------:| ----------------- |
|  id  | int10 |                | unsigned  | id                |
| acc  |   T   |                |  -------  | 帳號              |
|  pw  |   T   |                |  -------  | 密碼              |
|  pr  |   T   |                |  -------  | 權限<br>serialize |

---

> [!warning] 商品   ***$Goos => goods***     &nbsp; &nbsp; &nbsp; &nbsp;  總項目->11

| name  | type  | deffault value | attribute | remark                   |  
|:-----:|:-----:|:--------------:|:---------:| ------------------------ | 
|  id   | int10 |                | unsigned  | id                       |     
|  sn   |   T   |                |  -------  | 商品序號 <br> 隨機六位數 |     |
| name  |   T   |                |  -------  | 商品名稱                 |     
| price |   T   |                |  -------  | 商品價格                 |     
| spec  |   T   |                |  -------  | 商品規格                 |     
| stock |   T   |                |  -------  | 庫存量                   |     
|  img  |   T   |                |  -------  | 商品圖片                 |     
| intro |   T   |                |  -------  | 商品介紹                 |     
|  big  | int10 |                | unsigned  | 所屬大分類               |     
|  mid  | int10 |                | unsigned  | 所屬中分類               |     
|  sh   | int1  |       1        | unsigned  | 是否顯示(上架)           |     

> [!warning] 商品分類   ***$Type => type***  &nbsp; &nbsp; &nbsp; &nbsp;  總項目->3

|  name  | type  | deffault value | attribute | remark   |
|:------:|:-----:|:--------------:|:---------:| -------- |
|   id   | int10 |                | unsigned  | id       |
|  name  |   T   |                |  -------  | 商品名稱 |
| parent | int10 |                | unsigned  | 父層     |

#### 關於父層屬性 :
*  父層 0 為大分類   
*  父層 不為 0 時候，會等於 parent => 0 的 id ，
*  所以父層不為 0 時候為中分類

---

> [!note] 會員資料 ***$Mem => mem***  &nbsp; &nbsp; &nbsp; &nbsp; 總項目->8

| name  | type  | deffault<br>value | attribute | remark   |
|:-----:|:-----:|:-------------------:|:---------:| -------- |
|  id   | int10 |                     | unsigned  | id       |
| name  |   T   |                     |  -------  | 名稱     |
|  acc  |   T   |                     |  -------  | 帳號     |
|  pw   |   T   |                     |  -------  | 密碼     |
|  tel  |   T   |                     |  -------  | 電話     |
| addr  |   T   |                     |  -------  | 住址     |
| email |   T   |                     |  -------  | 電子信箱 |
| date  |   T   | current_timestamp() |  -------  | 註冊日期 | 

> [!note] 訂單 ***$Ord => ord***  &nbsp; &nbsp; &nbsp; &nbsp; 總項目->10

| name  | type  | deffault <br> value | attribute | remark                    |
|:-----:|:-----:|:-------------------:|:---------:| ------------------------- |
|  id   | int10 |                     | unsigned  | id                        |
|  sn   |   T   |                     |  -------  | 訂單編號:西元8位+隨機六位 |
| name  |   T   |                     |  -------  | 會員名稱                  |
|  acc  |   T   |                     |  -------  | 會員帳號                  |
|  tel  |   T   |                     |  -------  | 連絡電話                  |
| addr  |   T   |                     |  -------  | 註冊地址                  |
| email |   T   |                     |  -------  | 電子郵件                  |
| total |   T   |                     |  -------  | 商品總價                  |
| cart  |   T   |                     |  -------  | 商品內容  serialize       | 
| date  | date  | current_timestamp() |           |                           |

---


> [!important] 頁尾版權 ***$Bottom => bottom***  &nbsp; &nbsp; &nbsp; &nbsp; 總項目->3

| name | type  | deffault <br> value | attribute | remark   |
|:----:|:-----:|:-------------------:|:---------:| -------- |
|  id  | int10 |                     | unsigned  | id       |
| text |   T   |                     |  -------  | 頁尾內容 | 

> [!important] 廣告文字(最新消息) ***$Ad => ad***  &nbsp; &nbsp; &nbsp; &nbsp; 總項目->3

| name | type  | deffault <br> value | attribute | remark   |
|:----:|:-----:|:-------------------:|:---------:| -------- |
|  id  | int10 |                     | unsigned  | id       |
| text |   T   |                     |  -------  | 消息內容 | 
|  sh  | int1  |                     |           | 顯示與否 |

---

### 關於最新消息 :

#### html :
```php
<h2 class="ct">最新消息管理</h2>
<div class="ct"><button onclick="add()">新增消息</button></div>
<form action="api/news.php" method="post">
    <table class="all">
        <tr class="ct tt">
            <td>標題內容</td>
            <td>顯示</td>
            <td>操作</td>
        </tr>
        <?php
        $rows = $Ad->all();
        foreach ($rows as $row) {
            $sh = ($row['sh'] == 1) ? 'checked' : '';
        ?>
		<tr class="ct pp">
			<td>
				<input type="text" name="ads[]" value="<?= $row['text'] ?>">
			</td>
			<td>
				<input type="checkbox" name="sh[]" value="<?= $row['id'] ?>" <?= $sh ?>>
			</td>
			<td>
			  <button type="button" onclick="adDel(this,<?= $row['id'] ?>)">刪除</button>
			</td>
                <input type="hidden" name="id[]" value="<?= $row['id'] ?>">
		</tr>
        <?php } ?>
    </table>
    <div class='ct'>
        <input type='submit' value='編輯'> |
        <input type='reset' value='重置'>
    </div>
</form>
```
#### script :
```JS
    function add() {
        let div = `
				<tr class="ct pp">
					<td>
						<input type="text" name="add[]" >
					</td>
					<td>
						<input type="checkbox" name="sh[]" checked disabled="true">
					</td>
					<td>
						<button type="button" onclick="newDel(this)">刪除</button>
					</td>
				</tr>
                `;
        $('.all').append(div);
    }
    function adDel(dom, id) {
        $.post("api/ad_del.php", {id}, () => {
            $(dom).parents('tr').remove();
        })
    }
    function newDel(dom) {
        $(dom).parents('tr').remove();
    }

```
#### api/data
```php
<?php include_once "base.php";
if(isset($_POST['id'])){
    foreach($_POST['id'] as $key => $id){
        $Ad->save([
            'id'=>$id,
            'text'=>$_POST['ads'][$key],
            'sh'=>in_array($id,$_POST['sh'])?1:0,
        ]);
    }
}
if(isset($_POST['add'])){
    foreach($_POST['add'] as $add){
        if(trim($add)){
            $Ad->save(['text'=>$add]);
        }
    }
}
to("../admin.php?do=news");
```