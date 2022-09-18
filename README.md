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

+ `select * from 顧客 where 姓="稲富";`<br>

+ `select * from 顧客 where 姓="上野";`<br>

+ `select * from 顧客 where 生年月日="2005-07-06";` <br>

+ `select * from 顧客 where 生年月日>"2005-01-01";` <br>

+ `select * from 顧客 where 生年月日>="2005-01-01";` <br>

+ `select * from 顧客 where 生年月日<="2005-01-01";` <br>

+ `select * from 顧客 where 生年月日="1963-12-06";` <br>

+ `select * from 顧客 where 生年月日<>"1963-12-06";` (not equalのことで等しくないの意) 又は !=を使うが一般的ではない<br>

## 13. 複数の条件を指定しよう - and/or

+ `select * from 顧客 where 姓="上野" and 名="孝";`<br>

+ `select * from 顧客 where 姓="上野" or 名="孝";`<br>

+ `select * from 顧客 where 姓="上野" and not(名="孝");`<br>

 ```
select * from 顧客
    where (姓="上野" or 名="孝")
    and 生年月日="1961-02-13";
```

## 14. あいまい検索をしよう

+ `select * from 顧客 where 姓 like "上%";`<br>

+ `select * from 顧客 where 姓 like "%上";`<br>

+ `select * from 顧客 where 姓 like "%上%";`<br>

+ `select * from 顧客 where 姓 like "%上%" and 名="孝";`<br>

+ `select * from 顧客 where 姓 like "%上%" and 名 like "%孝%";`<br>

## 15. 並び替えをしよう - order by

+ `select * from 顧客;`<br>

※ `asc`の場合は省略可能<br>

+ `select * from 顧客 order by id desc;`<br>

+ `select * from 顧客 order by id asc;`<br>

+ `select * from 顧客 order by 生年月日 asc;`<br>

+ `select * from 顧客 order by 生年月日 desc;`<br>

+ `select * from 顧客 order by 姓;` この場合バラバラになる(漢字の場合は色々な読み方がある為文字コードの順番で検索している)<br>

+ `select * from 顧客 order by せい;` たまたまこの場合のデータは文字コード順に割り振られている為、正常に並び替えられる<br>

※ ひらがなとカタカナが混ざっている場合はひらがなの後にカタカナがくる なのでどちらかに統一した方が良い<br>

+ `select * from 顧客 order by せい, めい;`<br>

## 16. 件数を制限しよう - limit

+ `select * from 顧客;`<br>

+ `select * from 顧客 limit 10;`<br>

+ `select * from 顧客 limit 20, 10;` id21〜30に絞り込まれる<br>

+ workbenchの場合は`Preferences` => `SQL Excution` => `SELECT QUERY Result` => `Limit Rows`で自動的に絞られている。<br>

```
select * from 顧客 where 姓 like "上%" order by id desc
    limit 3; ## 順番厳守
```

# セクション3: 複数のテーブルを関連付けて利用しよう

## 17. リレーションの学習準備をしよう

```
create table 商品 (
    id int primary key auto_increment,
    カテゴリー varchar(45),
    商品名 varchar(1000),
    価格 smallint
);
```

+ workbrench => `File` => `Open SQL Script` => `items.sql`を選択 => `Open`<br>

+ `workbench`タブの`稲妻のマークをクリック`<br>

+ `select * from 商品;`<br>

## 18. カテゴリーを別のテーブルで管理しよう

```
create table カテゴリー (
    id int primary key auto_increment,
    カテゴリー名 varchar(45)
);
```

+ workbenchの`shop02` => `Tables` => `カテゴリーにマウスカーソルを当てると一番右の表ボタンが出るのでそれをクリック` => `Result Grid` => `Form Editor`をクリック => `カテゴリー名` => `洋服`と入力 => `Apply` => 再度`Apply`<br>

+ `select * from カテゴリー;`<br>

+ `Form Editor` => `1/2と表示されている横の右矢印をクリック` => `カテゴリー名` => `バッグ`と入力 => `Apply` => 再度`Apply`<br>

+ 3件目も同じように `カテゴリー名`に`グッズ`と登録する<br>

+ `update 商品 set カテゴリー=1 where カテゴリー="洋服";` safeモードによってエラーが出てしまうのでsafeモードを解除してreconnectして再度このクエリを実行する<br>

+ 商品テーブルの`カテゴリー`を`バッグ`を`2`に`グッズ`を`3`に変更する => `Apply`をクリックしておく<br>

+ `alter table 商品 modify column カテゴリー int;`<br>

+ `describe 商品`又は `desc 商品;` 現在の構成を確認<br>

## 19. 2つのテーブルを接続しよう - inner join

```
select * from 商品
    inner join カテゴリー
    on 商品.カテゴリー = カテゴリー.id;
```

+ `別の書き方`

```
select * from 商品, カテゴリー where
    商品.カテゴリー = カテゴリー.id;
```

```
select * from 商品
    inner join カテゴリー
    on 商品.カテゴリー = カテゴリー.id
    where カテゴリー.id = 1;
```

+ `別の書き方`

```
select * from 商品, カテゴリー where
    商品.カテゴリー = カテゴリー.id and
    カテゴリー.id = 1;
```

※ `on 商品.カテゴリー`の `商品`は省略可能(重複しないカラムのテーブルならOK、idの場合はどのテーブルのidなのかわからない為、省略できないことになる)<br>

```
select * from 商品
    inner join カテゴリー
    on カテゴリー = カテゴリー.id
    where カテゴリー.id = 1;
```

+ `別の書き方`

```
select * from 商品, カテゴリー where
    カテゴリー = カテゴリー.id and
    カテゴリー.id = 1;
```

## 20. ３つのテーブルを接続しよう

```
create table 購入履歴 (
    id int primary key auto_increment,
    顧客 int,
    商品 int,
    個数 int,
    購入日時 datetime
);
```

```
insert into 購入履歴(顧客, 商品, 個数, 購入日時) values(
    1,
    2,
    2,
    '2019-10-16 10:23:33'
);
```

```
select * from 購入履歴, 顧客, 商品 where
    顧客 = 顧客.id and
    商品 = 商品.id;
```

```
select 姓, 名, 商品名, 個数, 購入日時 from 購入履歴, 顧客, 商品 where
    顧客 = 顧客.id and
    商品 = 商品.id;
```

## 21. 関連付かないデータも表示したい - left-join / right-join

+ `truncate 購入履歴;`<br>

+ `history.sql`をインポートする<br>

```
select * from 顧客 inner join 購入履歴
    on 顧客.id = 顧客;
```

※ `inner join`は両方のテーブルにデータがない場合(null)は出力されない この場合 津島さんがそれである<br>

```
select * from 顧客 inner join 購入履歴
    on 顧客.id = 顧客
    order by 顧客.id;
```

* データがないもの(null)を出力させたい場合には`外部結合(left join = 左側のテーブルのデータは一旦全て表示しますの意)`を使う<br>

```
select * from 顧客 left join 購入履歴
    on 顧客.id = 顧客
    order by 顧客.id;
```

※ `right join` = 右側のテーブルのデータが全て表示されることになる<br>

```
select * from 顧客 right join 購入履歴
    on 顧客.id = 顧客
    order by 顧客.id;
```

```
select * from 購入履歴 right join 顧客
    on 顧客.id = 顧客
    order by 顧客.id;
```

```
select * from
    顧客 left join 購入履歴 on 顧客.id = 顧客
    inner join 商品 on 商品.id = 商品
    order by = 顧客.id;
```

```
select 姓, 名, 商品名, 個数, 購入日時 from
	顧客 left join 購入履歴 on 顧客.id = 顧客
    inner join 商品 on 商品.id = 商品
    order by 商品.id;
```

```
select 姓, 名, 商品名, 個数, 購入日時 from
	顧客 left join 購入履歴 on 顧客.id = 顧客
    left join 商品 on 商品.id = 商品
    order by 商品.id;
```