---
title: QT连接MYSQL
---

# 1.使用ODBC连接MYSQL

![1](/images/QT教程/QT连接MYSQL/ODBC.png)

输入 DATA SOURCE NAME 随机输入名字即可

TCP IP中输入端口号本机使用127.0.0.1：3306

User使用本机 root

密码和MYSQL中密码一致

DataBase使用创建好的数据库(不要点击下拉框！！！会卡死)

最后电机Test出现success即完成ODBC配置

# 2.在QT中编写代码连接数据库

```c++
#include <QVariant>
#include <QSqlDriver>
#include <QSqlRecord>
#include <QSqlField>
#include <QSqlError>
#include <qsqlquery.h>
```



```c++
QSqlDatabase db;
    if(QSqlDatabase::contains("my_qq")){
        db = QSqlDatabase::database("my_qq");
    }else{
        db = QSqlDatabase::addDatabase("QODBC","my_qq");
    }

    db.setHostName("127.0.0.1"); //IP
    db.setPort(3306); 、、端口号
    db.setDatabaseName("mysql"); //mysql连接
    db.setUserName("root");//UserName
    db.setPassword("******");//密码
    db.open();//打开MYSQL数据库
    QSqlQuery query = QSqlQuery(db);//可对数据库执行语句操作

     QString sql_1 = "SELECT * FROM `user`";
    query.exec(sql_1);

    while (query.next())
    {
        qDebug()<<query.value(0).toString();
        qDebug()<<query.value(1).toString();
        qDebug()<<query.value(2).toString();
    }

    db.close();//记得关闭哦！

    QString name;
    {
        name = QSqlDatabase::database().connectionName();
    }
    QSqlDatabase::removeDatabase("my_qq");
```

**出现DataBase Not Open的原因是用户名或者密码错误导致无法打开 **

**QSqlDatabasePrivate::removeDatabase: connection 'my_qq' is still in use, all queries will cease to work.出现原因目前没有找到，但是可以正常连接到数据库并进行操作**
