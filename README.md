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
