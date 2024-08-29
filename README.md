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
