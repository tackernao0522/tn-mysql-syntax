# セクション4: SQLの高度な機能を知ろう

## 22. 最大・最小・件数を集計していよう - max/min/count

+ `select * from 顧客;`<br>

+ `select 姓, 名 from 顧客;` <br>

+ 姓と名は繋げる場合 `select concat(姓, 名) from 顧客;` <br>

+ さらに せい と めい も繋げる `select concat(姓, 名), concat(せい, めい) from 顧客;` <br>

+ `select 姓 from 顧客;` <br>

+ `select 姓 from 顧客 order by 姓;` <br>

+ 重複を排除する `select distinct 姓 from 顧客 order by 姓;` <br>

+ `select * from 商品;` <br>

+ 消費税込みの価格の追加 `select *, 価格 * 1.1 from 商品;` <br>

+ 消費税込みの価格の追加 `select *, 価格 * 1.1 as 税込み金額 from 商品;` <br>

+ `select * from 購入履歴;` <br>

+ データの件数を知る `select count(*) from 購入履歴;`<br>

+ データの件数を知る `select count(*) as 履歴件数 from 購入履歴;`<br>

+ 合計を求める `select sum(個数) as 販売個数 from 購入履歴;` <br>

+ 平均を求める `select avg(個数) as 平均購入個数 from 購入履歴;`<br>

+ `select * from 商品;`<br>

+ 一番高額な商品はどれか？ `select max(価格) as 最高金額 from 商品;` <br>

+ 一番底額な商品はどれか？ `select min(価格) as 最低金額 from 商品;` <br>

## 23. 同じ種類で集計をまとめよう - group by

+ `select * from 購入履歴;`<br>

+ `select sum(個数) from 購入履歴;`<br>

+ 2番の商品が何個か? `select sum(個数) from 購入履歴 where 商品=2;` これだとidを一個ずつ調べていかなければならなくなる<br>

+ 商品ID毎の総個数の取得 `group by`を使う<br>

```
select 商品, sum(個数)
    from 購入履歴 group by 商品;
```

```
select 商品, avg(個数)
    from 購入履歴 group by 商品;
```

```
select 商品, count(個数)
    from 購入履歴 group by 商品;
```

```
select 商品, sum(個数)
    from 購入履歴 group by 商品
    order by 商品 asc;
```

+ リレーションをはる<br>

```
select 商品, 商品名, sum(個数)
    from 購入履歴 inner join 商品
    on 商品.id = 商品
    group by 商品
    order by 商品 asc;
```

+ where句を入れるときの順序<br>

```
select 商品, 商品名, sum(個数)
    from 購入履歴 inner join 商品
    on 商品.id = 商品
    where 商品 > 5
    group by 商品
    order by 商品 asc;
```

+ asをつける

```
select 商品, 商品名, sum(個数) as 合計
    from 購入履歴 inner join 商品
    on 商品.id = 商品
    group by 商品
    order by 商品 asc;
```

+ 合計を降順(大きい順)に並べ替える<br>

```
select 商品, 商品名, sum(個数) as 合計
    from 購入履歴 inner join 商品
    on 商品.id = 商品
    group by 商品
    order by 合計 desc;
```

## 24. 複雑な条件をいつでも使えるようにビューにしよう - view

+ view名はテーブル名やカラム名などど重複は不可<br>

```
create view 商品情報 as
    select 商品.id as 商品ID, カテゴリー名, 商品名, 価格
        from 商品 inner join カテゴリー
        on カテゴリー=カテゴリー.id;
```

+ refresh してみる<br>

+ `Views`ができている<br>

+ `select * from 商品情報;`<br>

```
select * from 購入履歴, 商品情報 where
    商品=商品ID;
```

+ このままだとエラーになる<br>

```
create view 商品情報 as
    select 商品.id as 商品ID, カテゴリー.id as カテゴリーID,
    カテゴリー名, 商品名, 価格
        from 商品 inner join カテゴリー
        on カテゴリー=カテゴリー.id;
```

+ 下記にする `create or replace`<br>

```
create or replace view 商品情報 as
    select 商品.id as 商品ID, カテゴリー.id as カテゴリーID,
    カテゴリー名, 商品名, 価格
        from 商品 inner join カテゴリー
        on カテゴリー=カテゴリー.id;
```

+ `select * from 商品情報;`<br>

+ `desc 商品情報`<br>

+ `drop view 商品情報;`<br>

## 25. 検索結果の中から、さらに検索をしよう - 副問い合わせ(サブクエリー)

+ `select * from 購入履歴;` <br>

```
select * from 購入履歴 inner join 商品
    on 商品=商品.id;
```

```
select *, 価格*個数 as 購入金額 from 購入履歴
    inner join 商品
    on 商品=商品.id;
```

+ 購入金額を集計毎で取りたい場合<br>

+ 下記はエラーになる<br>

```
select *, 価格*個数 as 購入金額, sum(購入金額) from 購入履歴
    inner join 商品
    on 商品=商品.id
    group by 商品;
```

```
select 商品, 価格*個数 as 購入金額 from 購入履歴
    inner join 商品
    on 商品=商品.id;
```

+ サブクエリーを使う<br>

```
select 商品, sum(購入金額) from
    (select 商品, 価格*個数 as 購入金額 from 購入履歴
    inner join 商品
    on 商品=商品.id) as 購入金額合計
    group by 商品;
```

+ 商品名もいれる<br>

```
select 商品, 商品名, sum(購入金額) from
    (select 商品, 商品名, 価格*個数 as 購入金額 from 購入履歴
    inner join 商品
    on 商品=商品.id) as 購入金額合計
    group by 商品;
```

## 26. 複数テーブルの検索結果をまとめて表示しよう - union

```
create table メルマガ顧客(
    id int primary key auto_increment,
    姓 varchar(10),
    名 varchar(10)
);
```

+ メルマガ顧客 テーブルに直接データ入力<br>

+ `姓` => `メルマガ`, `名` => `太郎`<br>

+ `select 姓, 名 from 顧客, メルマガ顧客;` エラーになる(どちらのテーブルのカラムなのか判断がつかない)<br>

+ `select 姓, 名 from 顧客;`<br>

+ `select 姓, 名 from メルマガ顧客;`<br>

+ unionの使用(両方のテーブルのデータを取得できる カラム名が互いに共通していなければならない 片方をidなどを追加することは不可)<br>

```
select 姓, 名 from 顧客
union
select 姓, 名 from メルマガ顧客;
```

+ union + where句<br>

```
select 姓, 名 from 顧客 where 姓="山田"
union
select 姓, 名 from メルマガ顧客;
```

+ メルマガ顧客に `山田` `周`を追加<br>

+ メルマガ顧客に入っている`山田 周`はヒットしない(unionは重複を取り除く)<br>

```
select 姓, 名 from 顧客 where 姓="山田"
union
select 姓, 名 from メルマガ顧客;
```

+ 両方表示させたい場合 `union all`<br>

```
select 姓, 名 from 顧客 where 姓="山田"
union all
select 姓, 名 from メルマガ顧客;
```
