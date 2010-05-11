MySQL
=====

    mysql -h 192.168.1.2 -u danil [-p] -t danil_test \
     < ~/src/common.sql > ~/data
    mysql -h localhost -u root [-p] \
     -e "SELECT * FROM test_db.test_table;"
    mysqlshow -h localhost -u root -p
    mysqladmin -h localhost -u root -p version
    mysql -u root -B -N \
     -e "SELECT DISTINCT
          CONCAT('SHOW GRANTS FOR ''',user,'''@''',host,''';')
          AS query FROM user" mysql | mysql -u root
    mysqlcheck -A

    CHECK TABLE ... EXTENDED;

Dump
----

`--default-character-set=latin1` option mean what MySQL server runing
with latin1 character set.  But data stored in database may be utf8.

    mysqldump -h 172.168.1.2 -u danil -p \
     --default-character-set=latin1 -r ~/backup.sql db_name \
     my_table, my_table_2

Show system variables
---------------------

    SHOW VARIABLES LIKE '%character%';

Create user
-----------

    GRANT CREATE, INSERT, DELETE, UPDATE, SELECT 
     ON danil.* TO 'danil'@'localhost' IDENTIFIED BY 'password';

Privileges: ALL, CREATE, DELETE, GRANT OPTION, INSERT, SELECT, SHOW
DATABASES, UPDATE, USAGE and so on
(<http://dev.mysql.com/doc/refman/5.1/en/grant.html>).

Set password
------------

    SET PASSWORD FOR 'danil'@'localhost' = PASSWORD('password');
    mysqladmin -u root -h localhost password 'new-password'

Show grants
-----------

    SHOW GRANTS;
    SHOW GRANTS FOR 'danil'@'localhost';
    SELECT Host, User FROM mysql.user WHERE User = 'danil';

Revoke grants
-------------

    REVOKE ALL ON danil.* FROM 'danil'@'localhost';

Remove user
-----------

    DELETE FROM mysql.user WHERE User = 'danil';

Show databases
--------------

    SHOW DATABASES LIKE '%danil%';
    +--------------------+
    | Database           |
    +--------------------+
    | danil              | 
    | information_schema | 
    | mysql              | 
    +--------------------+
    3 rows in set (0.00 sec)

Show character sets
-------------------

    SHOW CHARACTER SET LIKE 'utf%';
    +---------+---------------+-------------------+--------+
    | Charset | Description   | Default collation | Maxlen |
    +---------+---------------+-------------------+--------+
    | utf8    | UTF-8 Unicode | utf8_general_ci   |      3 | 
    +---------+---------------+-------------------+--------+
    1 row in set (0.00 sec)

Query
-----

Retrieve rows 6-15.

    SELECT Host, User, Password FROM mysql.user LIMIT 5,10;

### Query from file

    source sqlfile;

Create database
---------------

    CREATE DATABASE IF NOT EXISTS mobotix CHARACTER SET utf8;

Switching to database
---------------------

    USE some_db;

Alter database
--------------

    ALTER DATABASE 'mobotix_db_beta' DEFAULT CHARACTER SET utf8
     COLLATE utf8_general_ci;

Drop database
-------------

    DROP DATABASE IF EXISTS danil;

Describe database
-----------------

    SELECT * FROM INFORMATION_SCHEMA.SCHEMATA 
     WHERE SCHEMA_NAME LIKE 'danil';
    +--------------+-------------+----------------------------+
    | CATALOG_NAME | SCHEMA_NAME | DEFAULT_CHARACTER_SET_NAME |
    +--------------+-------------+----------------------------+
    | NULL         | danil       | utf8                       |
    +--------------+-------------+----------------------------+
    1 row in set (0.00 sec)

Create table
------------

    CREATE TABLE IF NOT EXISTS danil.test_table
      (
        id INT(11) NOT NULL AUTO_INCREMENT,
        text CHAR(255) NOT NULL,
        PRIMARY KEY (id)
      );
    CREATE TABLE IF NOT EXISTS danil.test_table_2
     LIKE danil.test_table;

Drop table
----------

    DROP TABLE IF EXISTS danil.test_table [, danil.test_table_2];

Show tables
-----------

    SHOW TABLES;
    SHOW FULL TABLES FROM danil LIKE 'test%';
    +-------------------------+------------+
    | Tables_in_danil (test%) | Table_type |
    +-------------------------+------------+
    | test_table              | BASE TABLE | 
    +-------------------------+------------+
    1 row in set (0.00 sec)

Describe table
--------------

    DESCRIBE test_table;
    SELECT * FROM INFORMATION_SCHEMA.TABLES
    WHERE table_schema = 'danil'
          AND table_name LIKE 'test_table';

Show table status
-----------------

    SHOW TABLE STATUS FROM danil LIKE 'test%';

Describe columns
----------------

    DESCRIBE danil.test_table '%';
    +------------+----------+------+-----+---------+----------------+
    | Field      | Type     | Null | Key | Default | Extra          |
    +------------+----------+------+-----+---------+----------------+
    | Id         | int(11)  | NO   | PRI | NULL    | auto_increment |
    | Name       | char(35) | NO   |     |         |                |
    | Country    | char(3)  | NO   | UNI |         |                |
    | District   | char(20) | YES  | MUL |         |                |
    | Population | int(11)  | NO   |     | 0       |                |
    +------------+----------+------+-----+---------+----------------+
    5 rows in set (0.00 sec)

Insert
------

    INSERT INTO danil.test_table (id, text) VALUES (1, 'test text');
    INSERT INTO tbl_temp2 (fld_id)
        SELECT tbl_temp1.fld_order_id
        FROM tbl_temp1 WHERE tbl_temp1.fld_order_id > 100;
    LOAD DATA LOCAL INFILE '~/path/to/filename' INTO TABLE table;


Delete row
----------

    DELETE FROM mysql.user WHERE User = 'danil';

Update columns
--------------

    UPDATE mobotix.menus SET article_text='' WHERE id='63';
    Query OK, 1 row affected (0.01 sec)