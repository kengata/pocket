# mysql
version情報
```
MACBOOK:pocket Ken$ mysql --version
mysql  Ver 14.14 Distrib 5.7.10, for osx10.9 (x86_64) using  EditLine wrapper
```





# 外部結合

stockのレコードをmeigara_mとuserテーブルと結合できない場合でも
表示するviewを追加
```
mysql> create view v_stock_JOIN as
    -> select t1.user_id,t3.name,t1.meigara_cd,t2.meigara_name,t1.zandaka,t1.ukewatashi_date,t2.torihiki_teishi
    -> from stock t1
    -> left join meigara_m t2 on t1.meigara_cd = t2.meigara_cd
    -> left join user t3 on t1.user_id = t3.user_id;
Query OK, 0 rows affected (0.01 sec)

mysql> show tables;
+-----------------+
| Tables_in_study |
+-----------------+
| meigara_m       |
| stock           |
| user            |
| v_stock         |
| v_stock_join    |
+-----------------+
5 rows in set (0.00 sec)

mysql> 
```





---
# シナリオ
以下のデータを管理するテーブルを作成し、データを登録する
	- 銘柄の情報
	- 銘柄コード、銘柄名、売買単位、取引停止など
- 預りの情報
	- 誰が、どの銘柄を、いつ買って、どれだけ持っているか
- 投資家の情報
	- 投資家の名前とか生年月日など

- それぞれのテーブルに必要最低限の情報をもたせる。
- 預りのテーブルに銘柄名や、投資家名は登録しない。
- 外部キーを設定する
- 預りの情報に銘柄名や投資家の名前を結合して表示する

meigara_m
銘柄の情報

stock
預りの情報

user
投資家の情報


## テーブル作成
- それぞれのテーブルに必要最低限の情報をもたせる。
- 預りのテーブルに銘柄名や、投資家名は登録しない。
- 外部キーを設定する
## create
### 預りTBL

ユーザーID、銘柄コード、残高、受渡日
```
mysql> create table stock2(
    -> user_id int,
    -> meigara_cd int,
    -> zandaka int,
    -> ukewatashi_date date,
    -> primary key(user_id,meigara_cd)
    -> );
Query OK, 0 rows affected (0.01 sec)

mysql> show full columns from stock2;
+-----------------+---------+-----------+------+-----+---------+-------+---------------------------------+---------+
| Field           | Type    | Collation | Null | Key | Default | Extra | Privileges                      | Comment |
+-----------------+---------+-----------+------+-----+---------+-------+---------------------------------+---------+
| user_id         | int(11) | NULL      | NO   | PRI | NULL    |       | select,insert,update,references |         |
| meigara_cd      | int(11) | NULL      | NO   | PRI | NULL    |       | select,insert,update,references |         |
| zandaka         | int(11) | NULL      | YES  |     | NULL    |       | select,insert,update,references |         |
| ukewatashi_date | date    | NULL      | YES  |     | NULL    |       | select,insert,update,references |         |
+-----------------+---------+-----------+------+-----+---------+-------+---------------------------------+---------+
4 rows in set (0.01 sec)
```

### 銘柄マスタ

銘柄コード、銘柄名、単位、取引停止日
```
mysql> create table meigara_m2(
    -> meigara_cd int primary key,
    -> meigara_name varchar(20),
    -> tanni int,
    -> torihiki_teishi date
    -> );
Query OK, 0 rows affected (0.01 sec)

mysql> show full columns from meigara_m2;
+-----------------+-------------+-----------------+------+-----+---------+-------+---------------------------------+---------+
| Field           | Type        | Collation       | Null | Key | Default | Extra | Privileges                      | Comment |
+-----------------+-------------+-----------------+------+-----+---------+-------+---------------------------------+---------+
| meigara_cd      | int(11)     | NULL            | NO   | PRI | NULL    |       | select,insert,update,references |         |
| meigara_name    | varchar(20) | utf8_general_ci | YES  |     | NULL    |       | select,insert,update,references |         |
| tanni           | int(11)     | NULL            | YES  |     | NULL    |       | select,insert,update,references |         |
| torihiki_teishi | date        | NULL            | YES  |     | NULL    |       | select,insert,update,references |         |
+-----------------+-------------+-----------------+------+-----+---------+-------+---------------------------------+---------+
4 rows in set (0.00 sec)
```

### 投資家TBL

ユーザーID、名前、契約日
```
mysql> create table user2(
    -> user_id int,
    -> name varchar(40),
    -> contract_date date,
    -> primary key(user_id)
    -> );
Query OK, 0 rows affected (0.03 sec)

mysql> show full columns from user2;
+---------------+-------------+-----------------+------+-----+---------+-------+---------------------------------+---------+
| Field         | Type        | Collation       | Null | Key | Default | Extra | Privileges                      | Comment |
+---------------+-------------+-----------------+------+-----+---------+-------+---------------------------------+---------+
| user_id       | int(11)     | NULL            | NO   | PRI | NULL    |       | select,insert,update,references |         |
| name          | varchar(40) | utf8_general_ci | YES  |     | NULL    |       | select,insert,update,references |         |
| contract_date | date        | NULL            | YES  |     | NULL    |       | select,insert,update,references |         |
+---------------+-------------+-----------------+------+-----+---------+-------+---------------------------------+---------+
3 rows in set (0.00 sec)
```


### 預りの情報に銘柄名や投資家の名前を結合して表示する

```
mysql> select t1.user_id,t3.name,t1.meigara_cd,t2.meigara_name,t1.zandaka,t1.ukewatashi_date
    -> from stock t1,meigara_m t2,user t3
    -> where t1.meigara_cd = t2.meigara_cd and t1.user_id = t3.user_id
    -> ;
+---------+-----------------+------------+--------------+---------+-----------------+
| user_id | name            | meigara_cd | meigara_name | zandaka | ukewatashi_date |
+---------+-----------------+------------+--------------+---------+-----------------+
|       1 | ケンシロウ      |       9001 | 京成電鉄     |    1000 | 2024-04-19      |
|       2 | ラオウ          |       9100 | 東武電鉄     |     500 | 2024-04-19      |
|       3 | トキ            |       8400 | 任天堂       |     600 | 2024-04-19      |
|       4 | ジャギ          |       2000 | 住友電工     |   10000 | 2024-04-19      |
|       5 | レイ            |       2000 | 住友電工     |    2000 | 2024-04-19      |
+---------+-----------------+------------+--------------+---------+-----------------+
```
### ビューを作る
クエリを保存しておくのに便利

stock表を軸にして、銘柄名を銘柄マスタから、名前をユーザー表からもってくるビュー

create view v_stock as
select t1.user_id,t3.name,t1.meigara_cd,t2.meigara_name,t1.zandaka,t1.ukewatashi_date
from stock t1,meigara_m t2,user t3
where t1.meigara_cd = t2.meigara_cd and t1.user_id = t3.user_id;

v_stockというビューを作成して、そのビューをselect

```
mysql> create view v_stock as
    -> select t1.user_id,t3.name,t1.meigara_cd,t2.meigara_name,t1.zandaka,t1.ukewatashi_date
    -> from stock t1,meigara_m t2,user t3
    -> where t1.meigara_cd = t2.meigara_cd and t1.user_id = t3.user_id;
Query OK, 0 rows affected (0.01 sec)

mysql> select * from v_stock;
+---------+-----------------+------------+--------------+---------+-----------------+
| user_id | name            | meigara_cd | meigara_name | zandaka | ukewatashi_date |
+---------+-----------------+------------+--------------+---------+-----------------+
|       1 | ケンシロウ      |       9001 | 京成電鉄     |    1000 | 2024-04-19      |
|       2 | ラオウ          |       9100 | 東武電鉄     |     500 | 2024-04-19      |
|       3 | トキ            |       8400 | 任天堂       |     600 | 2024-04-19      |
|       4 | ジャギ          |       2000 | 住友電工     |   10000 | 2024-04-19      |
|       5 | レイ            |       2000 | 住友電工     |    2000 | 2024-04-19      |
+---------+-----------------+------------+--------------+---------+-----------------+
5 rows in set (0.00 sec)
```

作成したビューはshow tablesコマンドでテーブルオブジェクトと同じように確認できる

```
mysql> show tables;
+-----------------+
| Tables_in_study |
+-----------------+
| meigara_m       |
| stock           |
| user            |
| v_stock         |
+-----------------+
4 rows in set (0.00 sec)
```

どれがview表か分からないので以下で
INFORMATION_SCHEMAデータベースのVIEWS表に情報がある。
show databasesで、スキーマ（＝データベース）を確認できる。
studyスキーマのviewが表示される。

```
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| study              |
| sys                |
| test               |
+--------------------+
6 rows in set (0.00 sec)
```

INFORMATION_SCHEMAデータベースのVIEWS表を表示。\Gは表示形式のオプション。
```
mysql> select * from INFORMATION_SCHEMA.VIEWS where TABLE_SCHEMA='study'\G;
*************************** 1. row ***************************
       TABLE_CATALOG: def
        TABLE_SCHEMA: study
          TABLE_NAME: v_stock
     VIEW_DEFINITION: select `t1`.`user_id` AS `user_id`,`t3`.`name` AS `name`,`t1`.`meigara_cd` AS `meigara_cd`,`t2`.`meigara_name` AS `meigara_name`,`t1`.`zandaka` AS `zandaka`,`t1`.`ukewatashi_date` AS `ukewatashi_date` from `study`.`stock` `t1` join `study`.`meigara_m` `t2` join `study`.`user` `t3` where ((`t1`.`meigara_cd` = `t2`.`meigara_cd`) and (`t1`.`user_id` = `t3`.`user_id`))
        CHECK_OPTION: NONE
        IS_UPDATABLE: YES
             DEFINER: root@localhost
       SECURITY_TYPE: DEFINER
CHARACTER_SET_CLIENT: utf8
COLLATION_CONNECTION: utf8_general_ci
1 row in set (0.00 sec)

ERROR: 
No query specified
```

いろいろ試してみる。

- stock表にレコード追加してからv_stockをselect
- cullent_date関数を利用
```
mysql> select * from stock;
+---------+------------+---------+-----------------+
| user_id | meigara_cd | zandaka | ukewatashi_date |
+---------+------------+---------+-----------------+
|       1 |       9001 |    1000 | 2024-04-19      |
|       2 |       9100 |     500 | 2024-04-19      |
|       3 |       8400 |     600 | 2024-04-19      |
|       4 |       2000 |   10000 | 2024-04-19      |
|       5 |       2000 |    2000 | 2024-04-19      |
+---------+------------+---------+-----------------+
5 rows in set (0.00 sec)

mysql> insert into stock values(1,2000,200,current_date);
Query OK, 1 row affected (0.02 sec)

mysql> select * from v_stock;
+---------+-----------------+------------+--------------+---------+-----------------+
| user_id | name            | meigara_cd | meigara_name | zandaka | ukewatashi_date |
+---------+-----------------+------------+--------------+---------+-----------------+
|       1 | ケンシロウ      |       2000 | 住友電工     |     200 | 2024-04-25      |
|       1 | ケンシロウ      |       9001 | 京成電鉄     |    1000 | 2024-04-19      |
|       2 | ラオウ          |       9100 | 東武電鉄     |     500 | 2024-04-19      |
|       3 | トキ            |       8400 | 任天堂       |     600 | 2024-04-19      |
|       4 | ジャギ          |       2000 | 住友電工     |   10000 | 2024-04-19      |
|       5 | レイ            |       2000 | 住友電工     |    2000 | 2024-04-19      |
+---------+-----------------+------------+--------------+---------+-----------------+

```
ビューに対して条件をつけて検索することができる
```
mysql> select * from v_stock where user_id = 1;
+---------+-----------------+------------+--------------+---------+-----------------+
| user_id | name            | meigara_cd | meigara_name | zandaka | ukewatashi_date |
+---------+-----------------+------------+--------------+---------+-----------------+
|       1 | ケンシロウ      |       2000 | 住友電工     |     200 | 2024-04-25      |
|       1 | ケンシロウ      |       9001 | 京成電鉄     |    1000 | 2024-04-19      |
+---------+-----------------+------------+--------------+---------+-----------------+
2 rows in set (0.01 sec)
```

stockにuserテーブルにないuser_idのレコードを追加する。
viewはuserテーブルと内部結合しているのでuserテーブルにないレコードは抽出されない。

user_id=6のレコードを追加
```
mysql> insert into stock values(6,9001,400,current_date);
Query OK, 1 row affected (0.01 sec)

mysql> select * from stock;
+---------+------------+---------+-----------------+
| user_id | meigara_cd | zandaka | ukewatashi_date |
+---------+------------+---------+-----------------+
|       1 |       2000 |     200 | 2024-04-25      |
|       1 |       9001 |    1000 | 2024-04-19      |
|       2 |       9100 |     500 | 2024-04-19      |
|       3 |       8400 |     600 | 2024-04-19      |
|       4 |       2000 |   10000 | 2024-04-19      |
|       5 |       2000 |    2000 | 2024-04-19      |
|       6 |       9001 |     400 | 2024-04-27      |
+---------+------------+---------+-----------------+
7 rows in set (0.00 sec)
```

userテーブル
```
mysql> select * from user;
+---------+-----------------+---------------+
| user_id | name            | contract_date |
+---------+-----------------+---------------+
|       1 | ケンシロウ      | 2024-04-21    |
|       2 | ラオウ          | 2024-04-20    |
|       3 | トキ            | 2024-04-19    |
|       4 | ジャギ          | 2024-04-18    |
|       5 | レイ            | 2024-04-21    |
+---------+-----------------+---------------+
```
viewの表示。userにuser_idが存在しないレコードは抽出されない。user_id=6が未選択。
```
mysql> select * from v_stock;
+---------+-----------------+------------+--------------+---------+-----------------+
| user_id | name            | meigara_cd | meigara_name | zandaka | ukewatashi_date |
+---------+-----------------+------------+--------------+---------+-----------------+
|       1 | ケンシロウ      |       2000 | 住友電工     |     200 | 2024-04-25      |
|       1 | ケンシロウ      |       9001 | 京成電鉄     |    1000 | 2024-04-19      |
|       2 | ラオウ          |       9100 | 東武電鉄     |     500 | 2024-04-19      |
|       3 | トキ            |       8400 | 任天堂       |     600 | 2024-04-19      |
|       4 | ジャギ          |       2000 | 住友電工     |   10000 | 2024-04-19      |
|       5 | レイ            |       2000 | 住友電工     |    2000 | 2024-04-19      |
+---------+-----------------+------------+--------------+---------+-----------------+

```

viewをstockテーブルを軸にした外部結合に変更する


---
#  ログイン

mysql -u root -p
pw
root

```
MACBOOK:pocket Ken$ mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 4
Server version: 5.7.10 MySQL Community Server (GPL)
```
---
# データベース一覧の表示

show databeses;

```
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| test               |
+--------------------+
5 rows in set (0.01 sec)
```
---
# データベースの選択

use データベース名;

```
mysql> use mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
```
---
# テーブル一覧の表示

show tables;

```
mysql> show tables;
+---------------------------+
| Tables_in_mysql           |
+---------------------------+
| columns_priv              |
| db                        |
| engine_cost               |
・・・
| time_zone_transition_type |
| user                      |
+---------------------------+
31 rows in set (0.00 sec)
```
