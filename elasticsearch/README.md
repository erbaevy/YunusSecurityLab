# Мониторинг безопасности Windows с помощью Elasticsearch, Kibana и Filebeat

## Содержание

- [1. Установка и запуск Elasticsearch](#1-установка-и-запуск-Elasticsearch-на-windows)
- [2. Интерфейс и запуск Kibana](#2-Установка-и-запуск-Kibana-на-Windows)
- [3. Установка Filebeat](#3-Установка-Filebeat)
- [4. Данные в Discover](#4-Первые-данные-в-Discover)
- [Вывод](#вывод)
- [Инструменты](#проект-выполнен-с-использованием-следующих-инструментов-и-технологий)


**В рамках проекта были реализованы:**

Установка и настройка Elasticsearch для хранения и поиска событий.

Установка и настройка Kibana для визуализации и анализа данных.

Создание и использование Enrollment Token для безопасного подключения Kibana к кластеру.

Установка Filebeat и настройка его как Windows Service для автоматического сбора логов.

Настройка источников данных для сбора системных журналов Windows.

Проверка поступления данных в Kibana → Discover.

Подтверждение работоспособности пайплайна от источника до визуализации.

## Ход работы

### 1. Установка и запуск Elasticsearch на Windows

1. Предварительное условие — наличие Java 17+.
Скачать можно с [OpenJDK 17 (Adoptium)](https://adoptium.net/temurin/releases/?version=17) и проверить установку:
```bash
java-version
```
2. Скачать [Elasticsearch](https://www.elastic.co/downloads/elasticsearch) и распаковать архив.

3. Запустить Elasticsearch:
```bash
cd C:\elastic\bin
.\elasticsearch.bat

```

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/elasticsearch/screenshots/1-elastic-launch.png)

4. Elasticsearch будет доступен по адресу:

```bash
https://localhost:9200

```

Интерфейс Elasticsearch

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/elasticsearch/screenshots/2-elastic-localhost.png)

5. При первом запуске в консоли выводится сгенерированный пароль пользователя elastic. Его необходимо сохранить для дальнейшей работы.

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/elasticsearch/screenshots/3-localhost.png)

### 2. Установка и запуск Kibana на Windows:

1. Скачать [Kibana](https://www.elastic.co/downloads/kibana) и распаковать.

2. Запустить

```bash
cd C:\kibana\bin
.\kibana.bat

```

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/elasticsearch/screenshots/4-kibana-launch.png)

3. Создать Enrollment Token для подключения Kibana к Elasticsearch:
   
```bash
elasticsearch-create-enrollment-token -s kibana

```

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/elasticsearch/screenshots/5-create-token.png)

4. Ввести токен в веб-интерфейсе для привязки к кластеру:


![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/elasticsearch/screenshots/6-kibana-token.png)

5. После успешного подключения отобразится страница входа:

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/elasticsearch/screenshots/7-kibana-localhost.png)

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/elasticsearch/screenshots/8-authorization.png)

### 3. Установка Filebeat

Filebeat используется для автоматического сбора событий Windows и отправки их в Elasticsearch.

1. Установить Filebeat.

```bash
.\install-service-filebeat.ps1
```

2. Запустить установку как Windows Service:

```bash
Start-Service filebeat

Get-Service filebeat

```

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/elasticsearch/screenshots/9-filebeat-install.png)

### 4. Первые данные в Discover

После запуска Filebeat в Kibana → Discover начали поступать события, что подтверждает корректную настройку пайплайна:

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/elasticsearch/screenshots/10-filebeat-kibana-elastic.png)

## Вывод

Развёрнут и настроен локальный Elastic Stack (Elasticsearch + Kibana + Filebeat).

Настроен сбор системных журналов Windows.

Обеспечена авторизация и защищённый доступ к данным.

Проверена корректность работы пайплайна через Discover.

## Проект выполнен с использованием следующих инструментов и технологий:

Elasticsearch — поисковая и аналитическая система для хранения и обработки событий.

Kibana — интерфейс визуализации данных и построения дашбордов.

Filebeat — агент для сбора логов с Windows и отправки их в Elasticsearch.

Windows Event Logs — встроенный источник событий безопасности и системных журналов.

PowerShell — консоль для управления сервисами и выполнения команд.

## Скриншоты
Все доказательства проделанной работы сохранены в папке screenshots.

## Автор проекта
Yunus Erbaev

GitHub: erbaevy










