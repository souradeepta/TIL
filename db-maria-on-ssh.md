# How to View MariaDB databases in SSH

When you are working with MariaDB to manage databases in cPanel, you will be working with the same interfaces as you did when working with MySQL. MariaDB is made to be a drop-in replacement for MySQL. In this tutorial, we will be showing a few commands using SSH to get to the database and view it.

1. [Login to SSH](https://www.inmotionhosting.com/support/website/ssh/shared-reseller-ssh).

2. Login to MySQL (MariaDB) using the database user that you know is assigned to the database you’re trying to view. the following command:

   `MariaDB [(none)]> mysql -u username -p`

   

   You will notice the following screen if you successfully login. Note that you can see that you’re using MariaDB at this point.

   [![Login to the mysql command line](https://www.inmotionhosting.com/support/wp-content/uploads/2017/07/mariadb-view_login-to-mysql.png)](https://www.inmotionhosting.com/support/wp-content/uploads/2017/07/mariadb-view_login-to-mysql.png)

   

   

3. There may be more than one database assigned to the user that you used to login to the mysql command line. Therefore, you need to select the database that will be used.

   `MariaDB [(none)]> show databases;`

   

   You will see a list of databases that are assigned to the user name that you are using.

   [![show the databases assigned to the user](https://www.inmotionhosting.com/support/wp-content/uploads/2017/07/mariadb-view_show-databases.png)](https://www.inmotionhosting.com/support/wp-content/uploads/2017/07/mariadb-view_show-databases.png)

   

   

4. At this point you need to select your database. Note that when you have selected the database your prompt will change to choose the database you have selected.

   `MariaDB [(none)]> use databasename;`

   

   [![Select the database you want to see](https://www.inmotionhosting.com/support/wp-content/uploads/2017/07/mariadb-view_select-db.png)](https://www.inmotionhosting.com/support/wp-content/uploads/2017/07/mariadb-view_select-db.png)

   

   

5. There are many commands involved in displaying the contents of a database. We’re going to be showing just a few of them:

   This will show a list of the tables from the database

   `MariaDB [arnel_test1]> show tables;`

   

   [![Select the database you want to see](https://www.inmotionhosting.com/support/wp-content/uploads/2017/07/mariadb-view_show-tables.png)](https://www.inmotionhosting.com/support/wp-content/uploads/2017/07/mariadb-view_show-tables.png)

   

   Show all the records in a specific table:

   `MariaDB [arnel_test1]> select * from databasetablename;`

   

   [![Select records from the database ](https://www.inmotionhosting.com/support/wp-content/uploads/2017/07/mariadb-view_show-records.png)](https://www.inmotionhosting.com/support/wp-content/uploads/2017/07/mariadb-view_show-records.png)

   

   In order to show all of the database table field names, type, and configuration of a certain table. Note that in order to view database specifics, you will need to have selected a database with the USE command.

   `MariaDB [arnel_test1]> describe databasetablename;`

   

   [![Describe database](https://www.inmotionhosting.com/support/wp-content/uploads/2017/07/mariadb-view_describe-table.png)](https://www.inmotionhosting.com/support/wp-content/uploads/2017/07/mariadb-view_describe-table.png)