# Шпаргалка по логам Proxmox

Краткий справочник по основным логам Proxmox VE и их использованию для диагностики.

## 1. Логи веб-интерфейса и API

**Файл:** `/var/log/pveproxy/access.log`

**Команда:**
```bash
tail -30 /var/log/pveproxy/access.log
Что показывает: каждый запрос к веб-интерфейсу и API Proxmox.

192.168.56.1 - - [13/Oct/2026:14:23:01 +0300] "POST /api2/json/nodes/pve/qemu/101/status/start HTTP/1.1" 200 45
Поле	Значение
192.168.56.1	IP клиента
[13/Oct/2026:14:23:01]	Дата и время
POST /api2/.../start	Метод и действие
200	HTTP-код (200 — успех, 401 — не авторизован, 500 — ошибка)
45	Размер ответа в байтах
Когда использовать:

Проверить, кто заходил в веб-интерфейс.

Найти неудачные попытки авторизации (401, 403).

Диагностировать ошибки API (500).

2. Логи демонов Proxmox
Команда:

bash
journalctl -u pvedaemon --since "1 hour ago" --no-pager
Основные сервисы:

Сервис	За что отвечает
pvedaemon	Управление VM и контейнерами
pveproxy	Веб-интерфейс и API
pvestatd	Сбор статистики (CPU, RAM, диски)
pve-cluster	Кластерная файловая система
Что искать:

starting task / end task OK — задача запущена и завершена успешно.

starting task без end task — задача зависла.

end task ... ERROR — задача провалилась.

authentication failure — неудачная попытка входа.

3. Логи задач
Каталог: /var/log/pve/tasks/

Команда:

bash
ls -lt /var/log/pve/tasks/ | head -10
Что показывает: список папок с логами задач (запуск VM, бэкап, миграция).

Как прочитать конкретную задачу:

bash
ls -la /var/log/pve/tasks/0/
cat /var/log/pve/tasks/0/UPID:pve:*
Альтернатива через веб-интерфейс: Datacenter → Tasks → клик на задачу.

4. Системные логи
Команда:

bash
journalctl -xe
Что показывает: все системные события — запуск сервисов, ошибки ядра, проблемы с дисками.

Фильтр по времени:

bash
journalctl --since "10 minutes ago"
journalctl --since today
🎯 Типовые сценарии диагностики
Симптом	Куда смотреть
VM не запускается	journalctl -u pvedaemon → ищите ERROR
Бэкап упал	Datacenter → Tasks → лог задачи
Подозрительная активность	/var/log/pveproxy/access.log → коды 401/403
Задача зависла	journalctl -u pvedaemon → starting task без end task
Медленная работа	Datacenter → Tasks → время между starting и end
Проблемы с диском	journalctl -xe + dmesg | grep -i error
