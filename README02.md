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
