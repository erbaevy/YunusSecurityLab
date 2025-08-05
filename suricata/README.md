# Демонстрация IDS Suricata

Этот проект демонстрирует базовую установку, настройку и тестирование IDS **Suricata** в виртуальной среде **Ubuntu**.

---

## Что я сделал

### 1. Установка Suricata
```bash
sudo apt install suricata -y
```

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/suricata/screenshots/1-install.png)

## 2. Обновление Suricata
```bash
sudo apt install suricata-update -y
```

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/suricata/screenshots/2-update.png)

### 3. Определение интерфейса и запуск
```bash
ip a
sudo suricata -i enp0s3
```

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/suricata/screenshots/3-start-listen.png)

### 4. Имитация атаки
```bash
curl http://testmyids.com
```
![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/suricata/screenshots/4-attack.png)


### 5. Проверка логов Suricata
```bash
tail -f /var/log/suricata/fast.log
```
![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/suricata/screenshots/5-check-logs.png)



