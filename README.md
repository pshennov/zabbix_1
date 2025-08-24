# Домашнее задание к занятию «Система мониторинга Zabbix» - 'Пшённов Николай'

## Задание 1
`Установите Zabbix Server с веб-интерфейсом.`

### Решение

* `Устанавливаю Postgres`
```
sudo apt update  
sudo apt postgresql
```
* `Устанавливаю репозиторий Zabbix`
```
wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.0+ubuntu24.04_all.deb
dpkg -i zabbix-release_latest_7.0+ubuntu24.04_all.deb
apt update 
```
* `Устанавливаю Zabbix сервер и веб-интерфейс`
```
sudo apt install zabbix-server-pgsql zabbix-frontend-php php8.3-pgsql zabbix-apache-conf zabbix-sql-scripts
```
* `Создаю базу данных`
```
sudo su - postgres -c 'psql --command "CREATE USER zabbix WITH PASSWORD '\'123456789\'';"'
sudo su - postgres -c 'psql --command "CREATE DATABASE zabbix OWNER zabbix;"
```
* `На хосте Zabbix сервера импортирую начальную схему и данные`
```
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix
```
* `Настройка базы данных для Zabbix сервера`
```
sudo nano /etc/zabbix/zabbix_server.conf
```
` Указывае в файле пароль`
```
DBPassword=123456789
```
* `Запускаю процессы Zabbix сервера и настраиваю их запуск при загрузке ОС. Проверяю статус`
```
sudo systemctl restart zabbix-server apache2
sudo systemctl enable zabbix-server apache2
sudo systemctl status zabbix-server apache2
```
* `В браузере, ввожу IP адрес сервера/zabbix, ввожу логин и пароль`
![Вход](https://github.com/pshennov/zabbix_1/blob/main/authorization.png)
![Панели](https://github.com/pshennov/zabbix_1/blob/main/panels.png)

## Задание 2
`Установите Zabbix Agent на два хоста.`

### Решение

* `Устанавливаю репозиторий Zabbix`
```
wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.0+ubuntu24.04_all.deb
dpkg -i zabbix-release_latest_7.0+ubuntu24.04_all.deb
apt update 
```
* `На два хоста устанавливаю zabbix-agent и проверяю его активность`
```
sudo apt install zabbix-agent
sudo systemctl status zabbix-agent
```
* `Редактирую файл /etc/zabbix/zabbix_agent.conf, добавив IP-адрес Zabbix-сервера`
```
nano /etc/zabbix/zabbix_agent.conf
```
```
Server=127.0.0.1,192.168.56.1
```
* `Перезапускаю агент, проверяю статус`
```
sudo nano restart zabbix_agentd.service
sudo nano status zabbix_agentd.service
```

* `Подключаю агенты к серверу`
![Хосты-1](https://github.com/pshennov/zabbix_1/blob/main/Hosts_1.png)
![Хосты-2](https://github.com/pshennov/zabbix_1/blob/main/Hosts_2.png)
* `Смотрю мониторинг`
![Мониторинг](https://github.com/pshennov/zabbix_1/blob/main/Latest_data.png)
* `Смотрю логи`
![Лог_1](https://github.com/pshennov/zabbix_1/blob/main/Log_22.04.png)
![Лог_2](https://github.com/pshennov/zabbix_1/blob/main/Log_24.04.png)

---

