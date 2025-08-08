# Мониторинг безопасности Windows с помощью Elasticsearch, Kibana и Filebeat

## Цель проекта

Продемонстрировать построение системы мониторинга безопасности на endpoint-уровне с использованием Elastic Stack.

Реализовать сбор и анализ журналов событий Windows через Filebeat.

Настроить базовые детекции по событиям безопасности (входы в систему, эскалация привилегий, запуск подозрительных процессов).

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
.\kibana.bat

```

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/elasticsearch/screenshots/4-kibana-launch.png)

3. Создать Enrollment Token для подключения Kibana к Elasticsearch:

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/elasticsearch/screenshots/5-create-token.png)

4. Ввести токен в веб-интерфейсе для привязки к кластеру:


![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/elasticsearch/screenshots/6-kibana-token.png)

5. После успешного подключения отобразится страница входа:

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/elasticsearch/screenshots/7-kibana-localhost.png)

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/elasticsearch/screenshots/8-authorization.png)

### 3. Установка Filebeat как сервиса

Filebeat используется для автоматического сбора событий Windows и отправки их в Elasticsearch.

1. Установить Filebeat.
2. Запустить установку как Windows Service:

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/elasticsearch/screenshots/9-filebeat-install.png)

### 4. Первые данные в Discover

После запуска Filebeat в Kibana → Discover начали поступать события, что подтверждает корректную настройку пайплайна:

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/elasticsearch/screenshots/10-filebeat-kibana-elastic.png)

## Вывод

Развёрнут и настроен локальный Elastic Stack (Elasticsearch + Kibana + Filebeat).

Настроен сбор системных журналов Windows.

Обеспечена авторизация и защищённый доступ к данным.

Проверена корректность работы пайплайна через Discover.

## Скриншоты
Все доказательства проделанной работы сохранены в папке screenshots.

## Автор проекта
Yunus Erbaev

GitHub: erbaevy










