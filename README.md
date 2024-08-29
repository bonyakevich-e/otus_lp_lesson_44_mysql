### OTUS Linux Professional Lesson #44 | Subject: MySQL репликация

#### ЦЕЛЬ: Настроить репликацию MySQL

#### Описание домашнего задания:

Развернуть базу данных на мастере, и реплицировать на слейв таблицы __bookmaker, competition, market, odds, outcome__.
Репликация должна выполнятся с помощью __GTID__

#### Процедура выполнения домашнего задания

Поднимает две виртуальные машины - master и slave. Master - основная БД, slave - реплика с master'а:
```
# vagrant up
```
Устанавливаем на обе машины __Percona Server for MySQL 5.7__
```
[root@master ~]# sed -i s/mirror.centos.org/vault.centos.org/g /etc/yum.repos.d/*.repo
[root@master ~]# sed -i s/^#.*baseurl=http/baseurl=http/g /etc/yum.repos.d/*.repo
[root@master ~]# sed -i s/^mirrorlist=http/#mirrorlist=http/g /etc/yum.repos.d/*.repo
[root@master ~]# yum install https://repo.percona.com/yum/percona-release-latest.noarch.rpm
[root@master ~]# percona-release setup ps57
[root@master ~]# yum install Percona-Server-server-57
```
Те же действия повторяем на slave.

По умолчанию Percona хранит файлы в таком виде:
- основной конфиг в /etc/my.cnf
- так же инклудится директория /etc/my.cnf.d/ - куда мы и будем складывать наши конфиги.
- data файлы в /var/lib/mysql

Копируем заранее подготовленные конфиги:
```
[root@master ~]# cp /vagrant/conf.d/* /etc/my.cnf.d/
```
Запускаем службу MySQL:
```
[root@master ~]# systemctl start mysql
[root@master ~]# systemctl status mysql
● mysqld.service - MySQL Server
   Loaded: loaded (/usr/lib/systemd/system/mysqld.service; enabled; vendor preset: disabled)
   Active: active (running) since Thu 2024-08-29 05:50:28 UTC; 9min ago
     Docs: man:mysqld(8)
           http://dev.mysql.com/doc/refman/en/using-systemd.html
  Process: 908 ExecStart=/usr/sbin/mysqld --daemonize --pid-file=/var/run/mysqld/mysqld.pid $MYSQLD_OPTS (code=exited, status=0/SUCCESS)
  Process: 598 ExecStartPre=/usr/bin/mysqld_pre_systemd (code=exited, status=0/SUCCESS)
 Main PID: 911 (mysqld)
   CGroup: /system.slice/mysqld.service
           └─911 /usr/sbin/mysqld --daemonize --pid-file=/var/run/mysqld/mysqld.pid

Aug 29 05:50:25 master systemd[1]: Starting MySQL Server...
Aug 29 05:50:28 master systemd[1]: Started MySQL Server.
```
Те же действия выполняем на slave.
