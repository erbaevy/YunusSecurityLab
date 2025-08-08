# Мониторинг безопасности Windows с помощью Filebeat + Elasticsearch + Kibana

## Цель проекта

Показать endpoint‑уровень: сбор журналов Windows без стороннего IDS.

Научиться работать с winlog‑входом в Filebeat, индексными шаблонами и Saved Objects в Kibana.

Реализовать детекции по событиям безопасности (входы в систему, эскалации, запуск подозрительных команд).

## Ход работы

### 1. Установка Elasticsearch и Kibana на Windows:

Elasticsearch требует Java 17+. Поэтому мы скачиваем Java с сайта [OpenJDK 17 (Adoptium)](https://adoptium.net/temurin/releases/?version=17) и проверяем через команду: 
```bash
java-version
```

Далее мы скачиваем Elastic по ссылке [Elasticsearch](https://www.elastic.co/downloads/elasticsearch) и запускаем его через команду: 
```bash
.\elasticsearch.bat
```

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/elasticsearch/screenshots/1-elastic-launch.png)

Он будет у нас доступен по адресу: https:localhost:9000. 

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/elasticsearch/screenshots/2-elastic-localhost.png)

Далее при первом запуске генерируется пароль и выводится прямо в консоли в ./elasticsearch.bat оттуда сохраняем Логин (обычно Elastic) и сгенерировшийся пароль и при входе у нас появляется такой интерфейс. 

Скриншот
