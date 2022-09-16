# セクション2: SQLの基本

## 4. SQLを体験しよう

+ `create database db01;` <br>

+ `Execute the statement`をクリック(ツールバー右から4番目)<br>

+ `下部のAction Output`に`create database db01`を表示されればOK<br>

+ `SCHEMAS`をRefreshすると`db01`ができているのが確認できる<br>

+ `use db01;` Execute the statementをクリック<br>

+ `create table members (name text);`を実行<br>

+ `SCHEMAS`をRefreshすると`Tables`の中に`members`tableが生成されている<br>

+ `insert into members set name="山田";`を実行<br>

+ `select name from members;`を実行<br>

## 5. データベーススペースを作成しよう - create database

+ `create database db02;`を実行<br>

+ `show databases;`を実行<br>

+ `drop database db02;`を実行<br>

+ `create database if not exists db03;` (省略しない書き方 既にあるdatabase名は作成しない)を実行<br>

+ 下記を実行<br>

```
create database if not exists db04
        character set=utf8mb4
        collate=utf8mb4_bin
        encryption='N'
        ;
```

+ `SCHEMAS`の dababase名の横の`i`マークをクリックするとそのdatabaseの詳細が確認できる<br>

+ `use db03;`を実行<br>

## 6. テーブルを作成しよう - create table

+ `create database shop;`<br>

+ `use shop;`<br>

+ 下記を実行<br>

```
create table 顧客情報
    (顧客名 text, 家族の人数 int, 入会日 date);
```

+ `show tables;`<br>

+ `desc 顧客情報;` or `describe 顧客情報;`<br>

+ `show columns from 顧客情報;` これも`desc`と同じ<br>

+ `drop table 顧客情報;`<br>

+ `if not exsits`を使用すると1回目は作成されるが2回目以降は作成されない(重複防止)<br>

```
create table if not exists 顧客情報
    (顧客名 text, 家族の人数 int, 入会日 date);
```

## 7. テーブルの構造を変更する - alter table

+ `alter table 顧客情報 add column 郵便番号 text;` `column`の部分は省略することができる<br>

+ `alter table 顧客情報 add 郵便番号2 text;`<br>

+ `alter table 顧客情報 add column カラム1 text first;`<br>

+ 下記を実行<br>

```
alter table 顧客情報
    add column カラム2 text after 顧客名;
```

+ `alter table 顧客情報 rename column 郵便番号 to 郵便;`<br>

+ `alter table 顧客情報 modify column 郵便 int;` 型の変更<br>

+ `alter table 顧客情報 modify column 郵便 int first;` 型は必ず記述する<br>

+ `alter table 顧客情報 drop column カラム1;`<br>

+ `alter table 顧客情報 drop column カラム2;`<br>

+ `alter table 顧客情報 drop column 郵便番号2;`<br>

## 8. データを挿入しよう

+ `drop tables 顧客情報;`<br>

```
create table if not exists 顧客情報
    (顧客名 text, 郵便番号 text, 家族の人数 int, 入会日 text);
```

+ 下記を実行<br>

```
insert into 顧客情報 values(
    '山田太郎',
    '123-4567',
    2,
    '2019-01-23'
);
```

+ `select 顧客名, 郵便番号, 家族の人数, 入会日 from 顧客情報;`<br>

or <br>

+ `select * from 顧客情報;`<br>

+ `insert into 顧客情報 (顧客名) values ('斎藤一');`<br>

+ `insert into 顧客情報 (郵便番号) values ('');` 空文字を入れてみる<br>

+ `alter table 顧客情報 modify 顧客名 text not null;` この時点ではエラーになる(既にNULLになっているデータがあるため)<br>

+ `truncate 顧客情報;` データを削除する<br>

+ `alter table 顧客情報 modify 顧客名 text not null;` <br>

+ `insert into 顧客情報 (郵便番号) values ('');` 空文字を入れてみる (NULLを許容しなくなった)<br>

+ `insert into 顧客情報 (顧客名) values ('斎藤一');` データが入る<br>

※ date型は文字列として入れる<br>

+ 下記を実行 (warningが出る)<br>

```
insert into 顧客情報 values(
    '山田太郎',
    '123-4567',
    2,
    '2019/01/23'
);
```

+ 下記を実行 (warningが出る)<br>

```
insert into 顧客情報 values(
    '山田太郎',
    '123-4567',
    2,
    '2019/01/23 01:12:23'
);
```

## 9. プライマリーキー（主キー）を設定しよう - primary key

+ `delete from 顧客情報;` safe modeにより削除できない<br>

+ `delete from 顧客情報 where 顧客名='山田太郎';`safe modeにより削除できない<br>

+ `alter table 顧客情報 add 顧客番号 int unique key;`<br>

+ `truncate 顧客情報;`<br>

```
insert into 顧客情報 (顧客名, 顧客番号)
        values('山田太郎', 1);
```

```
insert into 顧客情報 (顧客名, 顧客番号)
        values('佐藤一', 5);
```

```
alter table 顧客情報 modify 顧客番号 int primary key;
```

```
alter table 顧客情報 modify 顧客番号 int auto_increment;
```

```
insert into 顧客情報 (顧客名)
    values('山本紘一');
```

```
create table 商品 (
    商品ID int primary key auto_increment,
    商品名 text,
    価格 int
);
```

## 10. データの削除と変更をしよう

+ `delete from 顧客情報 where 顧客番号=1;`<br>

+ `update 顧客情報 set 郵便番号='123-4567';` safe modeによりエラーになる<br>

+ `update 顧客情報 set 郵便番号='123-4567' where 顧客番号=5;`<br>

+ `update 顧客情報 set 顧客名='佐山昭' where 顧客番号=5;`<br>

## 11. データをインポートしよう

+ `create database shop02;`<br>

+ `use shop02;`<br>

```
create table 顧客 (
    id int primary key auto_increment,
    姓 varchar(10) not null,
    名 varchar(10) not null,
    せい varchar(10) not null,
    めい varchar(10) not null,
    生年月日 date
);
```

+ 左から2番目のヘッダータブをクリック => `sample.sql`を選択 => `Open` <br>

+ `稲妻のマークをクリック`する<br>

+ `select * from 顧客;`<br>

## 12 . 操作するデータを特定しよう - where

+ `select * from 顧客 where id=465;`<br>

+ `select * from 顧客 where 姓="稲富";<br>

+ `select * from 顧客 where 姓="上野";<br>

+ `select * from 顧客 where 生年月日="2005-07-06"; <br>

+ `select * from 顧客 where 生年月日>"2005-01-01"; <br>

+ `select * from 顧客 where 生年月日>="2005-01-01"; <br>

+ `select * from 顧客 where 生年月日<="2005-01-01"; <br>

+ `select * from 顧客 where 生年月日="1963-12-06"; <br>

+ `select * from 顧客 where 生年月日<>"1963-12-06"; (not equalのことで等しくないの意) 又は !=を使うが一般的ではない<br>
