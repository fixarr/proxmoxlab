# Проблема с fail2ban в Proxmox

## Симптом
fail2ban не запускался, статус `inactive`, ошибка `File contains no section headers`.

## Причина
1. В Proxmox нет файла `/var/log/auth.log` (используется `systemd journal`).
2. В `jail.conf` параметр `backend` был объявлен дважды.

## Решение
1. В `/etc/fail2ban/jail.local` указать `backend = systemd` в секции `[DEFAULT]`.
2. Убедиться, что в `jail.conf` нет дублирующихся параметров.

## Проверка
```bash
systemctl status fail2ban  # Должно быть active (running)
fail2ban-client status     # Список активных джейлов
