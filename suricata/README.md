# Демонстрация IDS Suricata

Этот проект демонстрирует практическое развёртывание и тестирование системы обнаружения вторжений (IDS) — [Suricata](https://suricata.io) в изолированной среде на базе **Ubuntu**.

Проект включает в себя установку и обновление правил, запуск прослушивания трафика, симуляцию атаки и анализ сработавших сигнатур.

---

## Что я сделал

### 1. Установка пакета из репозитория APT и обновление сигнартур:

Используется встроенный механизм suricata-update для получения актуальных правил:
```bash
sudo apt install suricata -y
sudo apt install suricata-update -y
```

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/suricata/screenshots/1-install-update.png)

### 2. Запуск Suricata в режиме анализа трафика
Определение интерфейса и запуск IDS в live-режиме:

```bash
ip a
sudo suricata -i enp0s3
```

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/suricata/screenshots/3-start-listen.png)

### 3. Имитация вредоносной активности
Для генерации сигнатуры атаки используется тестовый домен:

```bash
curl http://testmyids.com
```
![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/suricata/screenshots/4-attack.png)


### 4. Анализ логов Suricata
После имитации атаки с использованием сайта http://testmyids.com, Suricata успешно зафиксировала подозрительный сетевой трафик. Для анализа срабатывания использовалась команда:

```bash
tail -f /var/log/suricata/fast.log
```
Зафиксированное событие:
![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/suricata/screenshots/5-check-logs.png)

| Поле                 | Значение                                                    |
| -------------------- | ----------------------------------------------------------- |
| **Дата и время**     | 08/05/2025 - 21:15:28.362883                                |
| **Сигнатура**        | `GPL ATTACK_RESPONSE id check returned root`                |
| **Классификация**    | `Potentially Bad Traffic` — потенциально вредоносный трафик |
| **Приоритет угрозы** | `2` (средний уровень)                                       |
| **Тип трафика**      | `TCP`                                                       |
| **Источник атаки**   | `217.160.0.187:80` (внешний адрес сайта testmyids.com)      |
| **Цель**             | `10.0.2.15:47994` (локальная виртуальная машина)            |

Suricata успешно выполнила свою функцию по обнаружению угроз. Сигнатура id check returned root указывает на попытку возврата данных, характерных для несанкционированного доступа (root) — это демонстрационный индикатор атаки.

## Вывод
В ходе лабораторной работы: 

Установлен и настроен Suricata IDS;

Загружены последние правила обнаружения угроз;

Выполнен мониторинг сетевого интерфейса;

Имитация атаки успешно определена системой;

Сработавшая сигнатура зафиксирована в логах.

## Используемые технологии

Suricata IDS

Ubuntu Linux

Тестовая нагрузка: testmyids.com

CLI: Bash, curl, tail

## Скриншоты
Все доказательства проделанной работы сохранены в папке screenshots.

## Автор проекта
Yunus Erbaev

GitHub: erbaevy
