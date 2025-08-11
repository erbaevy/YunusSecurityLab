# Демонстрация управления доступами в Active Directory в виртуальной среде

## Цель проекта
Цель проекта — развернуть тестовую инфраструктуру Active Directory и реализовать модель разграничения доступа на основе ролей (RBAC) и принципа минимальных привилегий.
Проект демонстрирует, как администратор может управлять пользователями и группами, предоставлять и ограничивать доступ к сетевым ресурсам, используя OU, группы безопасности, NTFS-права и GPO.

---

## Используемые инструменты

VirtualBox — виртуализация среды.

Windows Server 2022 — развертывание доменных служб Active Directory.

Windows 10 — рабочие станции, подключенные к домену.

Active Directory Domain Services (AD DS) — централизованное управление пользователями и группами.

Group Policy Management (GPO) — централизованная настройка параметров и автоматическое маппирование сетевых дисков.

NTFS — управление доступом на уровне файловой системы.

SMB — предоставление сетевого доступа к папкам. 

---

## Ход работы

### Подготовка виртуальной среды
Для реализации проекта была развернута тестовая инфраструктура в Oracle VirtualBox.
Были скачаны и установлены два ISO-образа:

Windows Server — для развертывания роли Active Directory Domain Services (AD DS) и настройки домена.

Windows 10 — для имитации рабочих станций, подключаемых к домену.

Обе виртуальные машины были объединены в одну виртуальную сеть, что позволило настроить взаимодействие между сервером и клиентами в изолированной среде.

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/active-directory/screenshots/0-vb-server-client.png)

---

### 1 Установка Windows Server и базовая настройка
- Установлен Windows Server 2022
- Изменено имя сервера → `AD-DC01`
- Настроен статический IP → `192.168.10.2`

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/active-directory/screenshots/1-iso-install.png)

---

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/active-directory/screenshots/2-ip-settings.png)


---

### 2 Развёртывание Active Directory
- Установлена роль **Active Directory Domain Services**
- Создан новый домен: `adlab.local`
- Сервер переведен в контроллер домена

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/active-directory/screenshots/3-adds.png)

---

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/active-directory/screenshots/4-adlab.png)


---

### 3 Создание OU и пользователей
- Созданы OU:
  - `Finance_Department`
  - `IT_Department`
  - `HR_Department`
- Добавлены пользователи:
  - `aibek.aibekov` (Finance)
  - `isken.iskenov` (Finance)
  - Пользователи из других отделов

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/active-directory/screenshots/5-create-ou.png)

---

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/active-directory/screenshots/6-create-group-ou.png)

---

### 4 Настройка общего ресурса (SMB)
- На сервере создана папка:  
  `C:\Departments\Finance`
- Папка расшарена через SMB
- NTFS-права:
  - **Finance_Department** — `Read/Write`
  - Остальные — `No Access`

 ![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/active-directory/screenshots/6-create-users-in-group.png)

---

### 5 Реализация RBAC (Role-Based Access Control)
- Для каждой группы назначены права только на свои ресурсы
- Принцип **минимальных привилегий** — доступ только к необходимым ресурсам

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/active-directory/screenshots/8-permission-folder-dep-fn.png)

---

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/active-directory/screenshots/10-security-finwrite.png)

---

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/active-directory/screenshots/11-aibekov-permission.png)

---

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/active-directory/screenshots/12-permission.png)

---

### 6 Создание GPO для маппинга диска
- Создана GPO: **Map-Finance-Drive**
- Привязана к OU `Finance_Department`
- В настройках GPO задано:
- Подключать сетевой диск `Z:` к `\\AD-DC01\Finance`

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/active-directory/screenshots/9-security-finread.png)

---

### 7 Подключение рабочих станций
- Windows 10 клиенты подключены к домену `adlab.local`
- Пользователи входят в систему под доменными учётными записями

  ![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/active-directory/screenshots/13-command-isken.png)
  
---

  ![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/active-directory/screenshots/14-test-iskenov.png)


---

### 8 Тестирование
- **aibek.aibekov** → диск Z: монтируется, чтение/запись доступны
- **isken.iskenov** → диск Z: монтируется, тест записи/удаления прошёл успешно
- Пользователи из других OU → диск Z: не отображается

---

## ✅ Результаты
- Централизованное управление доступами реализовано
- Принцип минимальных привилегий соблюден
- Подключение сетевых ресурсов автоматизировано через GPO

---

## 📷 Скриншоты
*(Вставьте свои скриншоты по шагам: установка сервера, создание OU, настройка SMB, GPO, тесты на клиентах)*

