# Домашнее задание к занятию "`Система мониторинга Zabbix`" - `Чумаков Константин`


### Инструкция по выполнению домашнего задания

   1. Сделайте `fork` данного репозитория к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/git-hw или  https://github.com/имя-вашего-репозитория/7-1-ansible-hw).
   2. Выполните клонирование данного репозитория к себе на ПК с помощью команды `git clone`.
   3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
      - впишите вверху название занятия и вашу фамилию и имя
      - в каждом задании добавьте решение в требуемом виде (текст/код/скриншоты/ссылка)
      - для корректного добавления скриншотов воспользуйтесь [инструкцией "Как вставить скриншот в шаблон с решением](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md)
      - при оформлении используйте возможности языка разметки md (коротко об этом можно посмотреть в [инструкции  по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md))
   4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`);
   5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
   6. Любые вопросы по выполнению заданий спрашивайте в чате учебной группы и/или в разделе “Вопросы по заданию” в личном кабинете.
   
Желаем успехов в выполнении домашнего задания!
   
### Дополнительные материалы, которые могут быть полезны для выполнения задания

1. [Руководство по оформлению Markdown файлов](https://gist.github.com/Jekins/2bf2d0638163f1294637#Code)

---
### Задание 1

Установите Zabbix Server с веб-интерфейсом.

# Решение

Установите PostgreSQL. 

Используемые команды:
```
sudo apt update
sudo apt install postgresql
```
Cоставил набор команд для установки последней версии Zabbix с поддержкой PostgreSQL и Apache.

![1](https://github.com/BudyGun/8-03-Zabbix/blob/main/img/zz5.png)


Установка репозитория Zabbix

```
wget https://repo.zabbix.com/zabbix/6.4/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.4-1+ubuntu22.04_all.deb
sudo dpkg -i zabbix-release_6.4-1+ubuntu22.04_all.deb
sudo apt update
```

Установка Zabbix сервер, веб-интерфейс и агент
```
sudo apt install zabbix-server-pgsql zabbix-frontend-php php8.1-pgsql zabbix-apache-conf zabbix-sql-scripts zabbix-agent
```

Выполняю следующие комманды на хосте, где распологается база данных.
```
sudo -u postgres createuser --pwprompt zabbix
sudo -u postgres createdb -O zabbix zabbix
```

На хосте Zabbix сервера импортирую начальную схему и данные. 

```
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix
```

Настроил базу данных для Zabbix сервера. Отредактировал файл 
```
sudo nano /etc/zabbix/zabbix_server.conf
```
DBPassword=12345


Запускаю процессы Zabbix сервера и агента и настраиваю их запуск при загрузке ОС.

```
systemctl restart zabbix-server zabbix-agent apache2
systemctl enable zabbix-server zabbix-agent apache2
```

![1](https://github.com/BudyGun/8-03-Zabbix/blob/main/img/zz1.png)
![1](https://github.com/BudyGun/8-03-Zabbix/blob/main/img/zz2.png)
![1](https://github.com/BudyGun/8-03-Zabbix/blob/main/img/zz3.png)
![1](https://github.com/BudyGun/8-03-Zabbix/blob/main/img/zz4.png)
