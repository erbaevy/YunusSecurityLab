# Практическая реализация IDS Suricata: установка, тестовые атаки и создание правил

## Содержание

- [1. Установка Suricata](#1-установка-пакета-из-репозитория-apt-и-обновление-сигнатур)
- [2. Запуск IDS](#2-запуск-suricata-в-режиме-анализа-трафика)
- [3. Имитация атаки через testmyids.com](#3-имитация-вредоносной-активности)
- [4. Анализ логов](#4-анализ-логов-suricata)
- [5. Настройка сети VirtualBox](#5-настройка-среды)
- [6. Имитация Nmap-сканирования](#6-имитация-разведывательной-активности-port-scanning)
- [7. Пользовательское правило](#7-создание-пользовательского-правила-signature)
- [Вывод](#вывод)
- [Инструменты](#проект-выполнен-с-использованием-следующих-инструментов-и-технологий)


Этот проект демонстрирует практическое развертывание и тестирование системы обнаружения вторжений (IDS) — [Suricata](https://suricata.io) в изолированной лабораторной среде на базе **Ubuntu Linux**.

**В рамках проекта были реализованы:**

Установка и базовая настройка Suricata;

Обновление правил обнаружения угроз через suricata-update;

Запуск Suricata в режиме мониторинга сетевого трафика;

Симуляция вредоносной активности с помощью тестовой нагрузки (testmyids.com);

Анализ сработавших сигнатур в логах (fast.log);

Имитация фазы разведки злоумышленника через nmap;

Создание и подключение собственного правила для обнаружения порт-сканирования;

Работа с логами и настройка среды VirtualBox для эмуляции атак.


---


## Ход работы

### 1. Установка пакета из репозитория APT и обновление сигнатур:

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

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/suricata/screenshots/2-start-listen.png)

### 3. Имитация вредоносной активности
Для генерации сигнатуры атаки используется тестовый домен:

```bash
curl http://testmyids.com
```
![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/suricata/screenshots/3-attack.png)


### 4. Анализ логов Suricata
После имитации атаки с использованием сайта http://testmyids.com, Suricata успешно зафиксировала подозрительный сетевой трафик. Для анализа срабатывания использовалась команда:

```bash
tail -f /var/log/suricata/fast.log
```
Зафиксированное событие:
![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/suricata/screenshots/4-check-logs.png)

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

### 5. Настройка среды

Для изоляции трафика использована **внутренняя сеть (Internal Network)** между Kali и Ubuntu в VirtualBox:

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/suricata/screenshots/5-kali-network.png)

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/suricata/screenshots/5-ubuntu-network.png)

### 6. Имитация разведывательной активности (Port Scanning)

Для демонстрации фазы разведки злоумышленника, с хост-машины Kali Linux было проведено скрытое TCP-сканирование всех портов целевой машины (Ubuntu с Suricata):

```bash
nmap -sS -p- 192.168.56.100
```

Сканирование выявило открытый порт: 9200/tcp

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/suricata/screenshots/6-nmap.png)

Suricata в этот момент вела мониторинг сетевого интерфейса. Однако Alert на порт-сканирование не был зафиксирован, что является нормой. Suricata по умолчанию не срабатывает на TCP SYN-сканирование, если отсутствует соответствующая сигнатура.

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/suricata/screenshots/7-ps.png)

### 7. Создание пользовательского правила (signature), чтобы Suricata начала фиксировать Nmap-сканирование.

Создание файла local.rules:
```bash
sudo nano /etc/suricata/rules/local.rules

```
Добавление правила:
```bash
alert tcp any any -> any any (msg:"Nmap Scan Detected"; flags:S; threshold:type both, track by_src, count 10, seconds 10; classtype:attempted-recon; sid:1000001; rev:1;)

```

Подключение local.rules в конфигурацию Suricata

```bash
sudo nano /etc/suricata/suricata.yaml

```
Проверка конфигурации 
```bash
sudo suricata -T -c /etc/suricata/suricata.yaml -v

```
Проверка логов 
```bash
sudo tail -f /var/log/suricata/fast.log

```

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/suricata/screenshots/8-practice.png)

| Поле                 | Значение                                                    |
|----------------------|-------------------------------------------------------------|
| **Дата и время**     | 08/06/2025 - 16:24:16.843015                                 |
| **Сигнатура**        | `Nmap Scan Detected`                                        |
| **Классификация**    | `Attempted Information Leak` — попытка утечки информации    |
| **Приоритет угрозы** | `2` (средний уровень)                                       |
| **Тип трафика**      | `TCP`                                                       |
| **Источник атаки**   | `192.168.56.101:59063` (Kali Linux)                         |
| **Цель**             | `192.168.56.100:587` (Ubuntu с Suricata)                   |


## Вывод

В рамках практического проекта была успешно развернута система IDS Suricata в изолированной виртуальной среде. Произведена установка и обновление правил, запуск анализа трафика, имитация атак и создание кастомных сигнатур. Suricata корректно зафиксировала как стандартную атаку через testmyids.com, так и сканирование портов после добавления пользовательского правила.


## Проект выполнен с использованием следующих инструментов и технологий:

Suricata IDS

Ubuntu Linux

Kali Linux (в роли атакующей машины)

Тестовая нагрузка: curl, testmyids.com, nmap

CLI-инструменты: bash, ip, tail, nano

Среда виртуализации: VirtualBox с внутренней сетью (Internal Network)

## Скриншоты
Все доказательства проделанной работы сохранены в папке screenshots.

## Автор проекта
Yunus Erbaev

GitHub: erbaevy
