# Демонстрация IDS Suricata

Этот проект демонстрирует базовую установку, настройку и тестирование IDS **Suricata** в виртуальной среде **Ubuntu**.

---

## Что я сделал

### 1. Установка Suricata
```bash
sudo apt install suricata -y

```
## 2. Обновление Suricata
```bash
sudo apt install suricata-update -y

```
### 3. Определение интерфейса и запуск
```bash
ip a
sudo suricata -i enp0s3

```
### 4. Имитация атаки
```bash
curl http://testmyids.com

```
### 5. Проверка логов Suricata
```bash
tail -f /var/log/suricata/fast.log

```
