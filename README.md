# セクション2: SQLの基本

## 4. SQLを体験しよう

+ `create database db01;` <br>

+ `Execute the statement`をクリック(ツールバー右から4番目)<br>

+ `下部のAction Output`に`create database db01`を表示されればOK<br>

+ `SCHEMAS`をRefreshすると`db01`ができているのが確認できる<br>

+ `use db01;` Execute the statementをクリック<br>

+ `create table mambers (name text);`を実行<br>

+ `SCHEMAS`をRefreshすると`Tables`の中に`members`tableが生成されている<br>

+ `insert into members set name="山田";`を実行<br>

+ `select name from members;`を実行<br>
