mysql 
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
# データベースの選択

use データベース名;

```
mysql> use mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
```
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

# クエリ

```
select count(*) from テーブル名;
```
