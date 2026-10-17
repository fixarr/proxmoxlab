ProxmoxLab

Домашняя лаборатория виртуализации для изучения Proxmox VE
## 🎯 Цель проекта
Создать рабочий стенд для отработки навыков диагностики, работы с LXC/KVM, настройки бэкапов и анализа логов в среде Proxmox VE.

## 🏗️ Стенд
- **Хост:** VirtualBox (NAT + Host-only Adapter)
- **Гипервизор:** Proxmox VE 9.2.21 (установлен внутри VirtualBox)
- **LXC-контейнер:** `test-ct` (Debian 12, nginx)
- **KVM-виртуалка:** `test-vm` (Debian 13.7, CPU `x86-64-v2-AES`, SCSI `VirtIO SCSI single`, сеть `VirtIO`)

## ✅ Что сделано
- [x] Установка и базовая настройка Proxmox VE в VirtualBox
- [x] Настройка сети (NAT для выхода в интернет, Host-only для доступа к веб-интерфейсу)
- [x] Защита веб-интерфейса с помощью `fail2ban`
- [x] Создание LXC-контейнера `CT100` с веб-сервером `nginx`
- [x] Создание KVM-виртуалки `test-vm` (Debian 13.7)
- [x] Настройка автоматического бэкапа для LXC и KVM
- [x] Изучение мониторинга в веб-интерфейсе (CPU, RAM, сеть, диски)
- [x] Разбор логов Proxmox (`pveproxy/access.log`, `journalctl -u pvedaemon`, `/var/log/pve/tasks/`)

## 📊 Сравнение LXC vs KVM

| Критерий | LXC (`CT100`) | KVM (`test-vm`) |
| :--- | :--- | :--- |
| **ОЗУ в покое** | ~40 МБ | ~120 МБ |
| **Время загрузки** | ~2 сек | ~5 сек |
| **Размер на диске** | ~1 ГБ | ~2 ГБ |
| **Изоляция** | Общее ядро | Своё ядро |
| **Тип ОС** | Только Linux | Любая |
| **Использование** | Лёгкие сервисы | Полноценные ОС |

## 📁 Структура репозитория
proxmox-lab/
├── README.md
├── notes/
│ ├── fail2ban-troubleshooting.md
│ ├── kvm-vm-startup-troubleshooting.md
│ └── proxmox-logs-cheatsheet.md
└── screenshots/

## 📝 Заметки и разборы
- [`notes/fail2ban-troubleshooting.md`](notes/fail2ban-troubleshooting.md) — Разбор проблемы с запуском `fail2ban`.
- [`notes/kvm-vm-startup-troubleshooting.md`](notes/kvm-vm-startup-troubleshooting.md) — Проблемы с запуском KVM-виртуалки в VirtualBox.
- [`notes/proxmox-logs-cheatsheet.md`](notes/proxmox-logs-cheatsheet.md) — Шпаргалка по логам Proxmox и типовым сценариям диагностики.

## 🚀 Что дальше
- Типовые сценарии диагностики
- Написание скриптов сбора диагностики
- Кластер Proxmox + PBS + мониторинг
