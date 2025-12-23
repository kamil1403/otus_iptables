<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/3/35/Tux.svg" alt="Linux Logo" width="100">
</p>

## ![Lesson](https://img.shields.io/badge/Lesson-iptables-00758F?style=for-the-badge&logo=linux&logoColor=white&labelColor=111827)![Author](https://img.shields.io/badge/Author-Kamil%20Ibragimov-10B981?style=for-the-badge&logo=github&logoColor=white&labelColor=111827)![Date](https://img.shields.io/badge/Date-23.12.2025-F59E0B?style=for-the-badge&logo=calendar&logoColor=white&labelColor=111827)

### 📌 Задание
1. Реализовать **Port Knocking**: SSH на роутере закрыт, открывается на 30 сек после "стука" по портам (7777 -> 8888 -> 9999).
2. Настроить **NAT (Port Forwarding)**: проброс порта 8080 на роутере в порт 80 на веб-сервере.
3. Полная автоматизация через **Ansible**.

### ✅ Результат
- [x] Стенд разворачивается одной командой (Vagrant + Ansible).
- [x] Port Knocking работает (скрипт `knock.sh` пробивает защиту).
- [x] NAT работает (страница отдается через проброшенный порт).

### 🧭 Оглавление
- [🧰 Шаг 1 - Инфраструктура](#one)
- [🛠️ Шаг 2 - Запуск](#two)
- [🔍 Шаг 3 - Проверка](#three)

---

<a id="one"></a>
## 🧰 Шаг 1 - Инфраструктура

Схема сети и адресация:

| ВМ | IP (Internal) | IP (Host-only) | Роль |
|----|--------------|----------------|------|
| **inetRouter** | 192.168.255.1 | - | Port Knocking (Firewall) |
| **inetRouter2** | 192.168.255.4 | 192.168.56.10 | NAT (8080 -> 80) |
| **centralRouter** | 192.168.255.2 | - | Клиент (тестирование стука) |
| **centralServer** | 192.168.255.3 | - | Web-сервер (Nginx) |

<a id="two"></a>
## 🛠️ Шаг 2 - Запуск
Топология описана в `Vagrantfile`, настройка — в `playbook.yml`.

Запуск стенда:
```bash
vagrant up
