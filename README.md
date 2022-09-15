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