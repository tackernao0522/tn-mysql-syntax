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
