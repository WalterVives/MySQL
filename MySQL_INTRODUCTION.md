# MySQL INTRODUCTION

## How to start?

install MySQL in Mac OS X:

open terminal an type:

```
waltervives$ brew install mysql
```
Then, I typed **mysql -u root -p** (mysql -u user_name -p) in my terminal but that does not work, I had to configurate the path.


```
waltervives$ sudo sh -c 'echo /usr/local/mysql/bin > /etc/paths.d/mysql'
```
_____________________________
To have access to MySQL first you need to start the MySQL server:

* Option 1:
	
	 Open system preferences -> click on MySQL -> click on Start Mysql Server.
* Option 2:

	 Configurate the path to start MySQL server from the 	terminal. before this I tried the command 	**mysql.server start** but does not work.
	 
	 You can create an alias in you .bash_profile to 	 start MySQL server.
	 
	 Open **~/.bash_profile** with your favorite text   	 editor.
	 
	```
	 waltervives$ vim ~/.bash_profile
	```
	
	 Then, create an alias typing **alias 	 alias_name=path** (you can write it in the bottom 	 of 	 the file): 
	 
	```
	alias startmysql="sudo launchctl load -F /Library/	LaunchDaemons/com.oracle.oss.mysql.mysqld.plist"
	 
	alias stopmysql="sudo launchctl unload -F /Library/	LaunchDaemons/com.oracle.oss.mysql.mysqld.plist"
	```	 	 
	 
	 
	 save the changes and refresh.
	 
	 bonus:
	 
	 You can also create an alias for your text editor,
	 in my case I created an alias for sublime text.
	 
	 ```
	 alias sub='open -a "Sublime Text"'
	 ```
	 Openning a file.
	 
	 ```
	 waltervives$ sub filename
	 
Well, we have configurated the path of **mysql -u root -p** and **MySQL Server**, it's time to initialize it.

After type **startmysql** the terminal will ask you for the password of your Mac.

After type **-u root -p** the terminal will ask you for the password of your user of MySQL.

```
waltervives$ startmysql
Password:
waltervives$ -u root -p
Enter password:
```
Note:

* You can type **mysql -u USER_NAME -pPASSWORD** and get access to MySQL, but the oficial documentation does not recommend this because if someone type the **history** command will be able to see you password, for that reason is recommended type **-u USER_NAME -p** and then you write the password.


Then, you will have access to MySQL.


```
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 10
Server version: 8.0.19 MySQL Community Server - GPL

Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
```
___________
We have seen how to start MySQL from the terminal, now let's see some commands.

Note:

* The keywords in MySQL can be written in upper case, it's optional, It's recommended write it in upper case to differentiate the keywords from the rest of the query.

* After write your query you have to put **;** at the end.

## MySQL commands

###Show the users

```
mysql> SELECT user FROM mysql.user;

```
There may be duplicate users. This is because MySQL filters access to a server according to the IP address it comes from. So you can also add a host column.

```
mysql> SELECT user, host FROM mysql.user;
```



###Create a database

```
mysql> CREATE database_name;
```

###Delete a database

```
mysql> DROP DATABASE database_name;
```

###Select a database

We need to select the database in which we want to work.

```
mysql> USE database_name;
```

###Create a table

```
mysql> CREATE TABLE table_name(column_name
 		datatype constraint default);
```
Also we can use:

```
mysql> CREATE TALBE IF NOT EXISTS 
		table_name(column_name datatype 
		constraint defalut);
```

example:

In this example we are only using the column_name and datatype.

```
mysql> CREATE TABLE clients(name varchar(30),
		lastname varchar(30), age int);  
```
Note:

* You can learn more about the datatypes in the official documentation of [MySQL](https://dev.mysql.com/doc/refman/5.6/en/data-types.html).

###Rename table

```
mysql> ALTER TABLE table_name RENAME new_name;
```

**or** 

The advantage of this one is that you can rename
multiple tables with a simple statement.

```
mysql> RENAME TABLE table_name TO new_name,
					table_name TO new_name,
					table_name TO new_name;
```
###Insert data into the table

We have different options to insert data into our table.

**Option 1.**

Especifying in which column you will insert the data.

```
mysql> INSERT INTO table_name(column_name, 
		another_column) VALUES(value1, value2);
```
Example:

Suposing that we have a table named *client* and the columns *name, lastname, age*.

```
mysql> INSERT INTO client(name, lastname, age) 
		VALUES("walter", "vives", 20);

```
If we don't have the data of *lastname* and the column *lastname* don't have the constraint NOT NULL, we can omit it.

```
mysql> INSERT INTO client(name, age) 
		VALUES("walter", 20);

```

**Option 2.**

```
mysql> INSERT INTO table_name VALUES(value1, vaule2);
```
Example:

Here we are inserting data into each column of the table client.

```
mysql> INSERT INTO client VALUES("walter", "vives", 20);
```
If we don't have the data of *lastname* and its constraint isn't NOT NULL.


```
mysql> INSERT INTO client VALUES("walter", null, 20);
```

**Option 3.**

Inserting data through a .txt


```
mysql> LOAD LOCAL DATA INFILE "path/file_name.txt"
		INTO TABLE table_name
		FIELDS TERMINATED BY ","
		ENCLOSED BY '"'
		LINES TERMINATED BY "\n"
		IGNORE n LINES;

```

Example:

```
mysql> LOAD LOCAL DATA INFILE "/Users/waltervives/
		desktop/clients.txt"
		INTO TABLE client
		FIELDS TERMINATED BY ","
		ENCLOSED BY '"'
		LINES TERMINATED BY "\n";
```
we don't used the IGNORE n LINES.

The file *clients.txt*:

```
"walter","vives",20
"carlos","hernandez",30
"Julio","ramirez",25
```
Then, the table *client* should look like:

```
mysql> SELECT * FROM client;
```
|name		       |lastname           |age             |
|----------------|:-----------------:|:--------------:|
|walter          |  vives            |         20     |
|carlos          |  hernandez        |         30     |
|julio           |  ramirez          |         25     |



Note: 

* If the table has a column that have the  constraint NOT NULL, you have to insert data into the respective column.

* You can research more about constraint in MySQL.

* You can read more about LOAD DATA Statement in [MySQL documentation](https://dev.mysql.com/doc/refman/8.0/en/load-data.html).


### Show how was created the table.

```
mysql> SHOW CREATE TABLE table_name;

```
_______________
Now could be that you are wondering, If I created my database through the command line, How can I save it in a file?

Open the terminal and type:

```
waltervives$ mysqldump -u user -p database_name > filename.sql
```
Then, the terminal will ask for your password and It's done :).

Note:

* You have to create an empty file and stay in the same place of the file before to execute the command.

How can I create an empty file through the terminal?


```
waltervives$ touch filename.sql
```



###Show the content of a table

```
mysql> SELECT * FROM table_name;
```
When we use __*__ in the query the result will show all the columns and rows of the table.

If you want to see only a few columns you can use:

```
mysql> SELECT column_name, column_name2, FROM
		 table_name;
```

You can use **LIMIT n** to only show **n** rows of your table, this is useful when you have a big table and you only want to see how look your table.

```
mysql> SELECT * FROM table_name LIMIT n;
```

Also, If you don't want to see n first lines you can use **OFFSET**.

```
mysql> SELECT * FROM table_name
			LIMIT N OFFSET N;
```

Example:

we have the table client.

|name		       |lastname           |age             |
|----------------|:-----------------:|:--------------:|
|walter          |  vives            |         20     |
|carlos          |  hernandez        |         30     |
|julio           |  ramirez          |         25     |
|maria           |  cervantes        |         17     |
|natalia         |  hernandez        |         23     |

And we want to see only see 2 rows and discart the 2 first rows.

```
mysql> SELECT * FROM client
		LIMIT 2 OFFSET 3;
```

output:


|name		       |lastname           |age             |
|----------------|:-----------------:|:--------------:|
|julio           |  ramirez          |         25     |
|maria           |  cervantes        |         17     |




###ORDER BY

```
mysql> SELECT * FROM table_name ORDER BY ASC/DESC;
```
ASC: ascending

DESC: descending


Example:

we have the table client.


|name		       |lastname           |age             |
|----------------|:-----------------:|:--------------:|
|walter          |  vives            |         20     |
|carlos          |  hernandez        |         30     |
|julio           |  ramirez          |         25     |

We want to order the rows by age from highest to lowest.


```
mysql> SELECT * FROM client ORDER BY age DESC;
```

output:

|name		       |lastname           |age             |
|----------------|:-----------------:|:--------------:|
|carlos          |  hernandez        |         30     |
|julio           |  ramirez          |         25     |
|walter          |  vives            |         20     |




###WHERE

The **where** statement It's useful to find certain information.


```
mysql> SELECT * FROM table_name WHERE condition(s);
```

The operators could be:

- < 

```
mysql> SELECT * FROM table_name WHERE 
		column_name < value;
```

- >

```
mysql> SELECT * FROM table_name WHERE 
		column_name > value;
```

- <=

```
mysql> SELECT * FROM table_name WHERE 
		column_name <= value;
```

- >=

```
mysql> SELECT * FROM table_name WHERE 
		column_name >= value;
```

- BETWEEN value1 AND value2

```
mysql> SELECT * FROM table_name WHERE 
		column_name BETWEEN value1 AND value2;
```

- NOT BETWEEN value1 AND value2

```
mysql> SELECT * FROM table_name WHERE 
		column_name NOT BETWEEN value1 AND value2;
```

- AND

```
mysql> SELECT * FROM table_name WHERE 
		column_name = value AND column_name = value2;
```

- OR

```
mysql> SELECT * FROM table_name WHERE 
		column_name = value or column_name= value2;
```
- IN


This operator works like the **OR** operator.

```
mysql> SELECT * FROM table_name WHERE 
		column_name IN(value1, value2, value3);
```

- NOT IN

```
mysql> SELECT * FROM table_name WHERE 
		column_name NOT IN(value1, value2, value3);
```


Example:

we have the table client.


|name		       |lastname           |age             |
|----------------|:-----------------:|:--------------:|
|walter          |  vives            |         20     |
|carlos          |  hernandez        |         30     |
|julio           |  ramirez          |         25     |



And we want to select the people that their age is less than 30.

```
mysql> SELECT * FROM client WHERE age < 30;
```

output:

|name		       |lastname           |age             |
|----------------|:-----------------:|:--------------:|
|walter          |  vives            |         20     |
|julio           |  ramirez          |         25     |

We can be more specific and ask only for the names that meet the condition.

```
mysql> SELECT name FROM client WHERE age < 30;
```

output:

|name  |
|------|
|walter|
|juilo |



###Show the tables of the database

```
mysql> SHOW TABLES;
```
**or**

```
mysql> SHOW FULL TABLES;
```
**or**

```
mysql> SHOW TABLES FROM database_name;
```
**or**

```
mysql> SHOW FULL TABLES FROM database_name;
```

###Show the total of rows in each table from a database

```
mysql> SELECT table_name, table_rows FROM 
		information_schema.tables WHERE 
		table_schema = "database_name";
```
In this query, the only change that we need to do is in **"database_name"**, the **table_name** and **table_rows** don't be changed.

output:

|table_name|table_rows|
|-------------|:-----:|
|table_name1  |	x	  |
|table_name2  |	x	  |
|table_name3  |	x	  |

###Show the total of tables of each database

```
mysql> SELECT table_schema, COUNT(table_schema) FROM 
	   information_schema.tables GROUP BY table_schema; 
```
In this query we don't have to change anything.

output:

|TABLE_SCHEMA    |COUNT(TABLE_SCHEMA)|
|----------------|:-----------------:|
|database_name1  |         x         |
|database_name2  |         x         |
|database_name 3 |         x         |


_________________________________________________
### Order of execution of a query

```
mysql> SELECT colum_name, column_name2
		FROM table_name1
		JOIN table_name2
		ON table_name1.coumn = table_name2
		WHERE conditions(s)
		GROUP BY column
		HAVING constraint_expresion
		ORDER BY column ASC/DESC
		LIMIT n OFFSET n;

```







