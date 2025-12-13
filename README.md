# Домашнее задание: Zabbix Server

## Скриншот дашборда

![Zabbix Dashboard](img/zabbix-dashboard.png)

## Команды установки Zabbix 7.0 LTS на Ubuntu 24.04
```bash
# Установка PostgreSQL
sudo apt update
sudo apt install -y postgresql

# Добавление репозитория Zabbix
wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.0+ubuntu24.04_all.deb
sudo dpkg -i zabbix-release_latest_7.0+ubuntu24.04_all.deb
sudo apt update

# Установка компонентов Zabbix
sudo apt install -y zabbix-server-pgsql zabbix-frontend-php php8.3-pgsql zabbix-apache-conf zabbix-sql-scripts zabbix-agent

# Создание базы данных
sudo -u postgres createuser --pwprompt zabbix
sudo -u postgres createdb -O zabbix zabbix
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix

# Настройка пароля БД в конфиге
sudo sed -i 's/# DBPassword=/DBPassword=zabbix123/' /etc/zabbix/zabbix_server.conf

# Запуск сервисов
sudo systemctl restart zabbix-server zabbix-agent apache2
sudo systemctl enable zabbix-server zabbix-agent apache2
```

## Доступ к веб-интерфейсу
- URL: http://localhost/zabbix
- Логин: Admin
- Пароль: zabbix
