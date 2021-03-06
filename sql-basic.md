
# [環境架設](https://hackmd.io/p/BypP-bAcQ)

--

# SQL基礎語法

---

## SQL是什麼呢？

--

一種用來跟關聯式資料庫(的管理程式)溝通的語言

--

![](https://i.imgur.com/OfUX6bI.gif)

--

## 關聯式資料庫？

--

就把他當成像 Excel 那樣用一張張的表來存資料

--

這種感覺

|學號|姓名|
|:--:|:--:|
|D0000000|王小明|
|D1234567|無名氏|

---

## 資料庫基本架構

--

資料庫(database)

裡面可以有好幾張表(table)

表裡面會有一張 n 欄的表格

--

像前面的表格可能就是個名叫 *Students* 的表

然後放在名叫 *FCU* 的資料庫底下

---

## 正題

---

### 基本語法 (一)

--

所以在上面提過的架構裡面

我們有 *資料庫*

--

於是就有了這樣的 SQL

- 建立 FCU 這個資料庫
  - ``CREATE DATABASE `FCU`;``

--

如何刪除

- 刪掉 FCU 這個資料庫
  - ``DROP DATABASE `FCU`;``

--

我們有了表

而表是屬於資料庫的

--

這邊可以先下 `use FCU;`

聲明我要用 `FCU`

--

之後再下

- 建立 Students 這個表
  - ``CREATE TABLE `Students` (stuid VARCHAR(10), name VARCHAR(255));``

--

如何刪除

- 刪掉 Students 這個表
  - ``DROP TABLE `Students`;``

--

也可以這樣用

- 在 `FCU` 建立 `Students` 這個表
  - ``CREATE TABLE `FCU`.`Students` (stuid VARCHAR(10), name VARCHAR(255));``

--

如何刪除

- 刪掉 `FCU` 裡的 `Students` 這個表
  - ``DROP TABLE `FCU`.`Students`;``

---

### 型態

--

#### 數字

--

##### 整數

- BIT (1bit)
- TINYINT / BOOL (8bit)
- MEDIUMINT (16bit)
- INT (32bit)
- BIGINT (64bit)

--

##### 帶小數點

- 固定小數點位數
  - DECIMAL/FIXED
- 浮點數
  - FLOAT
  - DOUBLE

--

#### 字串

--

##### 建立時指定長度(上限)

- 填滿長度
  - CHAR
    - 補空白
  - BINARY
    - 補 `'\0'`
- 不填滿長度
  - VARCHAR
  - VARBINARY

--

##### 不必指定長度上限的

- 以位元組為單位
  - TINYBLOB ( 255byte)
  - BLOB (~ 64kb)
  - MEDIUMBLOB (~ 16mb)
  - LONGBLOB (~ 4gb)
- 以對應編碼的一個字元為單位
  - TINYTEXT ( 255 chars)
  - TEXT (2^8-1 chars)
  - MEDIUMTEXT ( 2^16-1 chars )
  - LONGTEXT (2^32-1 chars)

---

### 基本語法 (二)

--

#### 查詢

--

`SELECT [DISTINCT] *|[欄位]... FROM 表;`

加上 DISTINCT 可以去掉重複的結果

--

`SELECT * FROM Students;`

倒出 `Students` 這張表裡的所有東西。

--

不過這邊還沒寫任何東西

所以你應該只會得到

![](https://i.imgur.com/lghEee3.png)

> 啥都沒有喔

--

#### 新增

- 不過為了能輸入中文所以我們要先做一點設定
- [Mac用戶看這裡](https://hackmd.io/p/rJimTF067#/)

--

![](https://i.imgur.com/tyC1AH2.png)

--

![](https://i.imgur.com/bEAPQql.png)

--

![](https://i.imgur.com/yr7LTtT.png)

--

在 `Students` 裡新增一個

學號 **D0000000** 名字是 **喵喵喵** 的人的資料

```sql
INSERT INTO Students (stuid,name)
      VALUES ( "D0000000", "喵喵喵" );
```

--

也可以只有學號

```sql
INSERT INTO Students (stuid) VALUES ("D0000001");
```

--

多加幾個

```sql
INSERT INTO Students (stuid) VALUES ("D0000002");
INSERT INTO Students (stuid) VALUES ("D0000003");
INSERT INTO Students (stuid) VALUES ("D0000004");
```

--

看看現在表裡有啥

--

```sql
SELECT * from Students;
```

![](https://i.imgur.com/aobNpLW.png)

--

#### 更新

--

剛剛加了幾個無名氏進來

幫 `D0000001` 加名字上去吧

叫他哈哈哈好了

--

但是這樣下會讓所有人都變哈哈哈

`UPDATE Students SET name="哈哈哈";`

--

所以我們需要個條件

--

只幫 stuid 是 D0000001 的那行更新名字

```sql
UPDATE Students SET name="哈哈哈" WHERE stuid="D0000001";
```

--

搭拉

```sql
SELECT * from Students;
```

![](https://i.imgur.com/tZ6t157.png)

--

順便把沒有名字的人都變成無名氏

```sql
UPDATE Students SET name="無名氏" WHERE name IS NULL;
```

--

無名氏們

```sql
SELECT * from Students;
```

![](https://i.imgur.com/bIdlOg5.png)

--

看看有哪些人名

```sql
SELECT DISTINCT name from Students;
```

![](https://i.imgur.com/XrGUjar.png)

---

#### 條件式

--

拿來限制 SQL 指令用的子句

`SELECT`, `UPDATE`, `REPLACE` 還有等等的 `DELETE` 適用

--

`LIKE` 是字串比較

```text
"abcd" LIKE "%bc%"
=> true

"acde" LIKE "%bc%"
=> false
```

--

```sql
SELECT * FROM Students WHERE stuid='D0000000';
```

![](https://i.imgur.com/vhhUMjG.png)

--

```sql
SELECT * FROM Students WHERE stuid='D0000000' or name='哈哈哈';
```

![](https://i.imgur.com/web5Q2E.png)

--

```sql
SELECT * FROM Students WHERE stuid='D0000000' and name='哈哈哈';
-- 啥都沒有
```

![](https://i.imgur.com/1e1AFa6.png)

--

#### 刪除

--

接下來講講怎麽刪

--

刪掉學號是 `D0000003` 這位同學好惹

```sql
DELETE FROM Students WHERE stuid='D0000003';
```

--

某無名氏掰

```sql
SELECT * FROM Students;
```

![](https://i.imgur.com/CsKaoSE.png)

--

手滑

```sql
DELETE FROM Students;
```

--

全掰

```sql
SELECT * FROM Students;
```

![](https://i.imgur.com/lghEee3.png)

--

下次要刪之前

記得先用同個 WHERE 子句 select 一次確認噢

---

### 註解

--

```sql
-- 單行註解 注意 -- 後要空白
select 1 AS A#單行註解;
/*註解*/
/*
多行註解
*/
```

雖然上面 `#` 那個沒變色但是他的確是註解

---

### 內建函式

--

- `USER()`
  - 取得目前使用者名稱
- `DATABASE()`
  - 取得目前開啟的資料庫名稱
- `SLEEP(n)`
  - 等待 n 秒後繼續執行
- `COUNT(n)`
  - 結果有幾個

--

- `SUBSTRING(str,start,len)`
  - 取 str 內從第 start 個字元開始的 len 個字元
  - `SUBSTRING("0123456789", 2, 3) #=> 123`
- `ASCII(str)`
  - 給出 str 第一個字的 ascii code
  - `ASCII("ABC") #=> 65`

--

講完瞜~
