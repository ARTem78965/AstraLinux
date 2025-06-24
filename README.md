# AstraLinux
Настройки сервисов на систему AstraLinux 

## НАСТРОЙКА VSFTPD.

```
# Настрока конфигурации /etc/vsftpd.conf.
# Выбрана работа по IPv4 и по IPv6
listen=YES
listen_ipv6=NO
# Анонимный доступ.
anonymous_enable=NO
# Вход локальных пользователей разрешен
local_enable=YES
# Включена выдача пользователям сообщения при входе в каталог
# (сообщения ищутся в файле .message)
dirmessage_enable=YES
# Отображать время в локальной временной зоне. По умолчанию - в GMT
use_localtime=YES
# Включено журналирование операций записи/скачивания.
xferlog_enable=YES
# Использовать 20-й порт для исходящих соединений (см. ниже)
# connect_from_port_20=YES
# Имя сервиса для авторизации через PAM
pam_service_name=vsftpd
# Размещение сертификатов для защищенных соединений
rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem
rsa_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key
ssl_enable=NO
# Раскомментировать, чтобы указать на использование файловой системы UTF8
utf8_filesystem=YES
# Индивидуальная настройка параметров пользователей
user_config_dir=/root/ftp_accept_users/

# Настройка индивидуального пользователя /root/ftp_accept_users/<Имя_пользователя>.
# Отдельный каталог с требуемым запретом или разрешением записи
local_root=/srv/ftp/disk1 # Пример пути.
# Пользователей к файловой системе по умолчанию будет ограничен назначаемым при входе рабочим каталогом (chroot-каталогом).
chroot_local_user=YES
# Разрешения на запись 
write_enable=YES
```

## НАСТРОЙКА SAMBA.
```
# Нстройки конфигурации /etc/samba/smb.conf
[global]
   # Рабочая группа.
   workgroup = WORKGROUP
   # Уровень безопастности сервера.
   security = user
   # Авторизация с неправильным паролем будет откланен.
   map to guest = bad user
   # Включение или выключение поддержку WINS.
   wins support = no
   # Возможность проксирование запросов по DNS.
   dns proxy = no
   # 
   bind interfaces only = yes
   # Выбор одного или несколько интерфейсов
   interfaces = wlan0 eth0   


[public]
   # Коментарий к директории.
   comment = "Доступ к публичной сетевой папке без авторизации"
   # Путь к дериктории.
   path = /srv/samba/public
   # Возможность доступа к каталогу без пароля (гостевой).
   guest ok = yes
   # Пользователь и группа.
   #force user = nobody
   #force group = nogroup
   # Отображена  или скрыта папка.
   browsable = yes
   # Разрешение на запись.
   writable = yes
   # Только чтение.
   read only = no


[private]
   # Коментарии к директории.
   comment = "Доступ к приватной сетевой папке с авторизацией"
   # Путь к директории.
   path = /srv/samba/private
   # Вылидация пользователь относящий к группе.
   valid users = @smbgrp
   # Пользователь или группа (если нужно дать доступ для одного пользователя)
   # force user = <Имя_пользователя>
   # force group = <Имя_группы>
   #Гостевой доступ
   guest ok = no
   # Отображена или скрыта папка
   browsable = yes
   # Разрешение на запись
   writable = yes
   # Только чтение 
   read only = no
```

# Если не получается подключиться к сетевому ресурсу через Windows, то используем коносль CMD:
* `net use z: \\<ip-адрес>\<имя_директории> /user:<имя_пользователь> <пароль>`