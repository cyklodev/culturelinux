# MariaDB
## Install
    mariadb_secure_install

## Root password 
### Hot change
    mysqladmin -u root -p'OLDPASSWORD' password 'NEWPASSWORD';
### Cold change
    systemctl mariadb stop
    mysqld_safe --skip-grant-tables &
    mysql mysql -u root
    UPDATE user SET password=PASSWORD('NEWPASSWORD') WHERE user="root";
    FLUSH PRIVILEGES;
    exit
    systemctl mariadb start

## User split
    mysqladmin -u root -p create MYDB
    mysql -u root -p mysql
    GRANT ALL ON MYDB.* TO MYUSER@'%' IDENTIFIED BY 'NEWPASSWORD';
    GRANT ALL ON MYDB.* TO MYUSER@localhost IDENTIFIED BY 'NEWPASSWORD';
    FLUSH PRIVILEGES;
    exit
* % : from any ip
* localhost: only from localhost

## Extract permissions
    use mysql
    select distinct concat('SHOW GRANTS FOR ''' , user, '''@''',host,''';') as query from user;
