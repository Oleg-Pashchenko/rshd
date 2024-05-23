Вариант: **`33161`**

## Задание:

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/3b73f928-92c4-4f11-8f9a-777be15fa67e/205802e7-1a16-455c-a6d4-95db0d795ac1/Untitled.png)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/3b73f928-92c4-4f11-8f9a-777be15fa67e/b8a96632-5910-4f75-a475-4671701b57b5/Untitled.png)

<aside>
🔑 Credentials: pg156:postgres5:Sj/erMgC

</aside>

## Этап #1

### Подключение:

```
ssh -J s335097@helios.cs.ifmo.ru:2222 -> подключение к helios 
ssh postgres5@pg156 -> подключение к postgres5
password: Sj/erMgC -> ввод пароля
```

### Инициализация кластера:

```
initdb -D $HOME/qrn46 --encoding=ISO_8859_5 --locale=ru_RU.ISO8859-5
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/3b73f928-92c4-4f11-8f9a-777be15fa67e/94bfe56c-600d-4645-b30e-5c6241bbe4c9/Untitled.png)

```
pg_ctl -D /var/db/postgres5/qrn46 -l server.journal start
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/3b73f928-92c4-4f11-8f9a-777be15fa67e/0c2352b8-3265-43e5-8c00-bb13304f7cba/Untitled.png)

## Этап #2

```
[postgres5@pg156 ~]$ cat $HOME/qrn46/postgresql.conf -> файл до изменений
```

https://github.com/Oleg-Pashchenko/rshd/blob/main/default.conf

### Требуется установить для **`postgres.conf`**:

```
# Конфигурация серверного подключения
port = 9161
listen_addresses = '*'

# Максимальное число соединений
max_connections = 8

# Память
shared_buffers = 512MB
temp_buffers = 64MB
work_mem = 64MB

# Контрольные точки
checkpoint_timeout = 15min
wal_buffers = 16MB
max_wal_size = 1GB

# Эффективный размер кэша
effective_cache_size = 2GB

# Безопасность записи
fsync = off
commit_delay = 100000

# Журналирование
log_destination = 'csvlog'
logging_collector = on
log_directory = '$HOME/ocl34'
log_filename = 'postgresql-%Y-%m-%d_%H%M%S.log'
log_truncate_on_rotation = on
log_rotation_age = 1d
log_rotation_size = 10MB
log_line_prefix = '%t [%p]: [%l-1] user=%u,db=%d,app=%a,client=%h '
log_connections = on
log_disconnections = on
log_min_messages = info
log_checkpoints = on
log_lock_waits = on
log_temp_files = 0

# WAL
wal_level = replica
archive_mode = on
archive_command = 'cp %p $HOME/ocl34/%f'

# Директория WAL файлов
data_directory = '$HOME/ocl34'

```

Файл после изменений: https://github.com/Oleg-Pashchenko/rshd/blob/main/postgresql-finish.conf

```
[postgres5@pg156 ~/qrn46]$ cat pg_hba.conf -> файл до изменений
```

https://github.com/Oleg-Pashchenko/rshd/blob/main/pg_hba-start.conf

### Требуется установить для **`pg_hba.conf`**:

```
# Unix-domain сокеты
local   all             all                                     peer

# TCP/IP подключения
host    all             all             0.0.0.0/0               password
host    all             all             ::/0                    password

```
