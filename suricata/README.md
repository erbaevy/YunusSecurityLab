# Демонстрация IDS Suricata

Этот проект демонстрирует практическое развёртывание и тестирование системы обнаружения вторжений (IDS) — [Suricata](https://suricata.io) в изолированной среде на базе **Ubuntu**.

Проект включает в себя установку, обновление правил, запуск прослушивания трафика, симуляцию атаки и анализ сработавших сигнатур.

---

## Что я сделал

### 1. Установка пакета из репозитория APT:
```bash
sudo apt install suricata -y
```

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/suricata/screenshots/1-install.png)

## 2. Обновление сигнатур
Используется встроенный механизм suricata-update для получения актуальных правил:

```bash
sudo apt install suricata-update -y
```

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/suricata/screenshots/2-update.png)

### 3. Запуск Suricata в режиме анализа трафика
Определение интерфейса и запуск IDS в live-режиме:

```bash
ip a
sudo suricata -i enp0s3
```

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/suricata/screenshots/3-start-listen.png)

### 4. Имитация вредоносной активности
Для генерации сигнатуры атаки используется тестовый домен:

```bash
curl http://testmyids.com
```
![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/suricata/screenshots/4-attack.png)


### 5. Анализ логов Suricata
Просмотр журнала событий fast.log, содержащего срабатывания сигнатур:

```bash
tail -f /var/log/suricata/fast.log
```
![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/suricata/screenshots/5-check-logs.png)

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
