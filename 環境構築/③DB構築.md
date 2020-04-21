# ③DB構築(MySQL)

***

※ 参考: https://qiita.com/nooboolean/items/7efc5c35b2e95637d8c1<br>
※ 参考2:  https://centossrv.com/mariadb.shtml

※ mysqlの保存先は、/usr/libexec/mysqld

----------------------------------------------------------------------------------------
## 1. mysql リポジトリを追加
* `sudo yum localinstall http://dev.mysql.com/get/mysql57-community-release-el7-7.noarch.rpm`
   - (※「release-el7-7」のelは、Enterprize Linux。7-7はCentOSのバージョン。)

## 2. mysql 関連パッケージをインストールする。
* ①を行ったので、`yum search`で、`mysql-community-server`が増えたのが確認できる。
  - `sudo yum install -y mysql-community-server`
  - ↑を実行することで、 「mysql-community-server, client, devel, libs, libs-compat, common」が<br>
      依存ライブラリなので、勝手にインストールされる。(yumの特性です。rpmはしてくれません。)

## 3. 起動する
* `systemctl statr mysqld`実行後、`systemctl status mysqld`で起動したか確認する。
  - `systemctl enable mysqld`でスタートアップ対象になります。

## 4. ログインする(mysqlに接続する)
* `mysql -u root`
   - ※ しかし、パスワードが設定されてないので怒られる。
   - ※ その後、自動生成されてログに出力されます。
      > #=> ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: NO)

## 5. 自動生成されたパスワードを確認
* ログ保存先に移動
   - `cd /var/log/`
* ログからpasswordを確認
   - `cat mysqld.log | grep --color password`
      > #=> 2019-03-21T08:00:56.605953Z 1 [Note] A temporary password is generated for root@localhost: `XXXXXXX`

## 6. 今度こそログイン
* `mysql -u root`を入力後、`XXXXXXX`を入力

## 7. ルートパスワードを設定 (※ 8文字以上 && 4種類以上の文字)
* ログインした後、`SET PASSWORD = PASSWORD('Password')`を実行。

## 8. 文字コードを設定 
* まずは確認する
   ```bash
   mysql> show variables like "character_set%";
   ```
   
   ```
    +--------------------------+----------------------------+
    | Variable_name            | Value                      |
    +--------------------------+----------------------------+
    | character_set_client     | utf8                       |
    | character_set_connection | utf8                       |
    | character_set_database   | latin1                     |
    | character_set_filesystem | binary                     |
    | character_set_results    | utf8                       |
    | character_set_server     | latin1                     |
    | character_set_system     | utf8                       |
    | character_sets_dir       | /usr/share/mysql/charsets/ |
    +--------------------------+----------------------------+
    ```
      ※ character_set_client     : クライアント側で発行したsql文はこの文字コードになる
      ※ character_set_connection : クライアントから受け取った文字をこの文字コードへ変換する
      ※ character_set_database   : 現在参照しているDBの文字コード
      ※ character_set_results    : クライアントへ送信する検索結果はこの文字コードになる
      ※ character_set_server     : DB作成時のデフォルトの文字コード
      ※ character_set_system     : システムの使用する文字セットで常にutf8が使用されている

* 設定する。
   - mysqlからログアウトして、設定ファイルを編集する。
      `sudo vi /etc/my.cnf`←ファイル名が`mysql.cnf`じゃなくて`my.cmf`らしいので注意
   - 下記を追加
      `character_set_server=utf8`
      `skip-character-set-client-handshake`

## 9. サーバーを再起動して、確認する
   - `systemctl restart mysqld` (※ 動かなかった際は、設定ファイルの編集をミスってる)

* 確認
   `mysql> show variables like 'character%';`
     
   ```
     +--------------------------+----------------------------+
     | Variable_name            | Value                      |
     +--------------------------+----------------------------+
     | character_set_client     | utf8                       |
     | character_set_connection | utf8                       |
     | character_set_database   | utf8                       |
     | character_set_filesystem | binary                     |
     | character_set_results    | utf8                       |
     | character_set_server     | utf8                       |
     | character_set_system     | utf8                       |
     | character_sets_dir       | /usr/share/mysql/charsets/ |
     +--------------------------+----------------------------+
    ```

### 10. ユーザを作成
* 全DBへのアクセス権限を持ったユーザーを作成。(rootユーザーで作成)
  `grant all privileges on test.* to user_name@localhost identified by 'user_pwd';`

* 確認
  `select user from mysql.user`

* 権限確認
  `show grants;`
   ```
	+-------------------------------------------------------------------------+
	| Grants for web_conn@localhost                                           |
	+-------------------------------------------------------------------------+
	| GRANT ALL PRIVILEGES ON *.* TO 'web_conn'@'localhost' WITH GRANT OPTION |
	| GRANT ALL PRIVILEGES ON `test`.* TO 'web_conn'@'localhost'              |
	+-------------------------------------------------------------------------+
   ```

## 11. スキーマ(DB)を作成
※ 作成したユーザーで、再ログインを行います。
  
* 作成
  `create database webdb;`

* 確認
  `show databases;`

* 使用するスキーマを指定
  `use webdb;`


## 12. テーブルを作成
   ```sql
	create table user (
    	id varchar(255) primary key not null,
    	password varchar(8) not null,
    	status tinyint(2) not null,
    	created_at datetime not null
  	);
   ```
   
   ※ クエリの確認
       `show create table user;`

   ※ カラムの確認
      `show fields from user;`
      
      ```
	+------------+--------------+------+-----+---------+-------+
	| Field      | Type         | Null | Key | Default | Extra |
	+------------+--------------+------+-----+---------+-------+
	| id         | varchar(255) | NO   | PRI | NULL    |       |
	| password   | varchar(8)   | NO   |     | NULL    |       |
	| status     | tinyint(2)   | NO   |     | NULL    |       |
	| created_at | datetime(2)  | NO   |     | NULL    |       |
	+------------+--------------+------+-----+---------+-------+
      ```
***

### DB操作メモ

◆ バージョンの確認
→ 「select varsion();」

◆ DBの選択
→ 「use dbName;」

=========================
◇ テーブル操作

◆ フィールド(カラム) 情報表示 (データ型、NULL okなどなど)
→ 「show columns from tableName」


◆ データベースの削除
→ 「drop database databaseName;」


◆ テーブルの削除
→ 「drop table tableName;」


◆ テーブル内容の確認
→ 「select * from tableName;」
   (※「id,name」という風に書けば、選択要素を指定できる。Railsの「permit(:id, :name)」に似てる)
   (※ この場合「*」は、「テーブル内の全て」を参照)


◆ テーブルのDDLの表示
→ 「show create table table_name\G」 ※ Gはなんだか調べるように！

◆ フィールド(カラム)の追加
→ 「ALTER TABLE table_name ADD column_name smallint DEFAULT 0 NOT NULL」

※ 末尾以外の位置にカラム追加 (SQLServerにはない)
→ 「ALTER TABLE table_name ADD column_name smallint DEFAULT 0 NOT NULL AFTER exist_column_name」


◆ フィールド情報の修正、追加
→ 「alter table posts add columnName char(64) not null;」
→ 「alter table posts add columnName timestamp not null default CURRENT_TIMESTAMP;」


◆ フィールド名の変更
→ 「alter table posts rename COLUMN old_colmun new_column:」


◆ フィールド情報の変更
→ 「ALTER TABLE posts modify columnName char(64) NOT NULL;」


◆ フィールド名と、フィールド情報の変更
→ 「ALTER TABLE login_campaigns CHANGE old_column new_column varchar(300) NOT NULL;」


◆ フィールド情報(UNIQUE制約)の削除
→ 「alter table posts drop index user_id;」


◆ フィールドの削除
→ 「alter table posts drop columnName;」


◆ テーブルから抽出したデータを、別名に変更する
→ 「select count(*) from users AS user_counts;」


◆ テーブルから抽出したデータの列名を、別名に変更する
→「select user_id AS mc_id from users;」 ※ mc...MyConstance


◆ インデックスを追加する
→「CREATE INDEX idx_orders_admin_user_id ON lunch_orders (admin_user_id ASC);」
			or
→「ALTER TABLE lunch_orders ADD INDEX idx_orders_admin_user_id (admin_user_id ASC);」


◆ テーブルをコピーする
1. 空のテーブルを作成
   → create table users2 like users;

2. 元テーブルのデータをインサートする
   → insert into users2 select * from users;

=========================
◇ データ操作

◆ データの挿入
「insert into tableName (column_name1, column_name2) values(1, 'BayaSea');」。
※ カラムにデフォルト値が設定されていた場合、「values(DEFAULT, 'BayaSea')ともかける。」

◆ データの更新
「update tableName set columnName = 'hoge' where id <= 5」
※ idが5以下のデータの「name」を、'hoge'に変える。 
※ (複数行の更新も可能。 set columnName1='hoge', columnName2=0xffffffff )


update stream_infos set created_at += '09:00:00';

◆ データの削除
「delete from tableName where id=1;」 ※ idが1のデータが削除される。
「detete from tableName;」           ※ データをすべて削除する。テーブルは削除されない。

※ 期間指定
「delete from lunch_order_logs where (order_date <= "2019-08-26");」


=========================
◇ データの結合(join)
※「ON」は、条件。
※ innerは、「user.id = item.id」の場合、「idが同じレコード」のみを取得する。

・テーブル「user」
+------+-----------+
| id   | name      |
+------+-----------+
|    1 | BayaSea   |
|    2 | BayaSeeee |
|    3 | aaaa      |
|    4 | takeSea   |
|    5 | takaSea   |
|    6 | error!!   |
|    7 | error!!   |
|    8 | error!!   |
|    9 | D.Takada  |
|   10 | error!!   |
| NULL | NULL      |
|   11 | NULL      |
+------+-----------+

・テーブル「item」
+------+-----------+
| id   | item_name |
+------+-----------+
|    1 | gold      |
|    2 | sword     |
|    3 | sield     |
|    4 | iphone    |
|    5 | surfece   |
|    6 | iphone5   |
|    7 | iphone5s  |
|    8 | iphone6   |
|    9 | iphone6s  |
|   10 | iphoneX   |
+------+-----------+
=========================
◇ 内部結合「inner join」
「select user.id, user.name, item.item_name from tableName1 inner join tableName2 on tableName1.id = tableName2.id;」

→この場合、「一致したレコード」のみ取得できる。
+------+-----------+-----------+
| id   | name      | item_name |
+------+-----------+-----------+
|    1 | BayaSea   | gold      |
|    2 | BayaSeeee | sword     |
|    3 | aaaa      | sield     |
|    4 | takeSea   | iphone    |
|    5 | takaSea   | surfece   |
|    6 | error!!   | iphone5   |
|    7 | error!!   | iphone5s  |
|    8 | error!!   | iphone6   |
|    9 | D.Takada  | iphone6s  |
|   10 | error!!   | iphoneX   |
+------+-----------+-----------+

=========================
◇ 外部結合
※ outerは、「user.id = item.id」の場合、
「idが同じレコード」+ 左 or 右にで指定したテーブルのすべてのレコードを取得できる。

◆ 左外部結合「left  outer join」
「select * from tableName1 left outer join tableName2 on tableName1.id=tableName2.id;」

→この場合、「一致したレコード」+ leftに指定したuserテーブルの「全レコード」を取得する。(※ NULLで一致してないけど、表示してるよ!!!)
+------+-----------+-----------+
| id   | name      | item_name |
+------+-----------+-----------+
|    1 | BayaSea   | gold      |
|    2 | BayaSeeee | sword     |
|    3 | aaaa      | sield     |
|    4 | takeSea   | iphone    |
|    5 | takaSea   | surfece   |
|    6 | error!!   | iphone5   |
|    7 | error!!   | iphone5s  |
|    8 | error!!   | iphone6   |
|    9 | D.Takada  | iphone6s  |
|   10 | error!!   | iphoneX   |
| NULL | NULL      | NULL      |
|   11 | NULL      | NULL      |
+------+-----------+-----------+



◆ 右外部結合「right outer join」
「select * from tableName1 right outer join tableName2 on tableName1.id=tableName2.id;」

→この場合、「一致したレコード」+ rightに指定したテーブルの「全レコード」を取得する。
(※ rightのitemテーブルのカラムは10個しかないので、それ以下は消えたよ!!!)

+------+-----------+-----------+
| id   | name      | item_name |
+------+-----------+-----------+
|    1 | BayaSea   | gold      |
|    2 | BayaSeeee | sword     |
|    3 | aaaa      | sield     |
|    4 | takeSea   | iphone    |
|    5 | takaSea   | surfece   |
|    6 | error!!   | iphone5   |
|    7 | error!!   | iphone5s  |
|    8 | error!!   | iphone6   |
|    9 | D.Takada  | iphone6s  |
|   10 | error!!   | iphoneX   |
+------+-----------+-----------+

----------------------------------
◇ より快適なDB操作を実現するために...

※ indexを設定すると、性能が劇的に上がることがあります！


◆ テーブルの情報(実行計画)を見る ... 「explain select * from tableName;」を使う。
→ 「explain select * from user where name='BayaSea';」

※ インデックスの使用状況などが分かる。参考: https://qiita.com/kzbandai/items/ea02727f4bb539fcedb5
※ Extraに、使われた検索方法が書かれる。
+------+-------------+-------+------+---------------+------+---------+------+------+-------------+
| id   | select_type | table | type | possible_keys | key  | key_len | ref  | rows | Extra       |
+------+-------------+-------+------+---------------+------+---------+------+------+-------------+
|    1 | SIMPLE      | user  | ALL  | NULL          | NULL | NULL    | NULL |   12 | Using where |
+------+-------------+-------+------+---------------+------+---------+------+------+-------------+


◆ インデックスの設定を見る ... 「show index from tableName;」

※ Key_nameがインデックスの名前。
+-------+------------+------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
| Table | Non_unique | Key_name   | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment |
+-------+------------+------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
| user  |          1 | user_index |            1 | id          | A         |          12 |     NULL | NULL   | YES  | BTREE      |         |               |
+-------+------------+------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+

----------------------------------
◇ グループ操作

・productsテーブル
+------+-------------+-------+
| id   |  category   | value |
+------+-------------+-------+
|    1 |           1 |     5 |
+------+-------------+-------+
|    2 |           2 |     4 |
+------+-------------+-------+
|    3 |           1 |     1 |
+------+-------------+-------+
|    4 |           1 |     2 |
+------+-------------+-------+
|    5 |           2 |     1 |
+------+-------------+-------+


> select count(*) from products;
#=> 6

※ categoryのデータ毎に1グループとして、テーブルを作成
> SELECT category, SUM(value) FROM products GROUP BY category;
#=> 
+-------------+-------+
|  category   | value |
+-------------+-------+
|           1 |     8 |
+-------------+-------+
|           2 |     5 |
+-------------+-------+
