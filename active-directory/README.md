# Демонстрация управления доступами в Active Directory в виртуальной среде

## Цель проекта
Развернуть и протестировать инфраструктуру на базе Windows Server 2022 с ролью Active Directory, реализовать принципы минимальных привилегий и RBAC (Role-Based Access Control), подключить клиентскую Windows к домену и проверить работу разграничения прав доступа.

## Описание проекта
Проект выполнен в изолированной виртуальной среде на базе VirtualBox.

Были установлены два виртуальных компьютера:

Windows Server 2022 — контроллер домена, хранит учетные записи, группы, политики безопасности и сетевые ресурсы.

Windows 10 — клиентская рабочая станция, подключенная к домену.

В процессе работы:

Настроен домен adlab.local.

Созданы организационные единицы (OU) для разных отделов.

Реализованы группы безопасности и назначены права.

Настроено автоматическое подключение сетевых ресурсов через GPO.

Проведено тестирование доступа для разных учетных записей.

---

## Ход работы

### 0. Подготовка виртуальной среды

Загружены два ISO-образа и созданы две ВМ в VirtualBox: **Windows Server 2022** (для AD) и **Windows 10** (клиент).  

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/active-directory/screenshots/0-vb-server-client.png)

---

### 1. Установка Windows Server и базовая настройка
* Установлен Windows Server 2022 (ISO от Microsoft).  

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/active-directory/screenshots/1-iso-install.png)

- Изменено имя сервера → `AD-DC01`
- Настроен статический IP → `192.168.10.2`

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/active-directory/screenshots/2-ip-settings.png)

---

### 2. Развёртывание Active Directory (AD DS) и создание домена

* Через **Server Manager** установлена роль **Active Directory Domain Services**.  

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/active-directory/screenshots/3-adds.png)

---

* Сервер промотирован до контроллера домена; создан лес/домен **`adlab.local`**.  

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/active-directory/screenshots/4-adlab.png)

---

### 3. Организация структуры каталога и учеток

* Созданы **OU** для отделов: `Finance_Department`, `IT_Department`, `HR_Department`.  

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/active-directory/screenshots/5-create-ou.png)

---

* В OU **Finance_Department** созданы группы безопасности:  
  - **Fin_Read** — чтение;  
  - **Fin_Write** — чтение/запись.

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/active-directory/screenshots/6-create-group-ou.png)

---

* Созданы пользователи и добавлены в группы:  
  - **aibek.aibekov** → `Fin_Read`  
  - **isken.iskenov** → `Fin_Write`

 ![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/active-directory/screenshots/7-create-users-in-group.png)

---

### 4 Реализация RBAC (Role-Based Access Control)
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

### 5 Создание GPO для маппинга диска
- Создана GPO: **Map-Finance-Drive**
- Привязана к OU `Finance_Department`
- В настройках GPO задано:
- Подключать сетевой диск `Z:` к `\\AD-DC01\Finance`

![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/active-directory/screenshots/9-security-finread.png)

---

### 6 Подключение рабочих станций
- Windows 10 клиенты подключены к домену `adlab.local`
- Пользователи входят в систему под доменными учётными записями

  ![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/active-directory/screenshots/13-command-isken.png)
  
---

  ![123](https://github.com/erbaevy/YunusSecurityLab/blob/main/active-directory/screenshots/14-test-iskenov.png)


---

### 7 Тестирование
- **aibek.aibekov** → диск Z: монтируется, чтение/запись доступны
- **isken.iskenov** → диск Z: монтируется, тест записи/удаления прошёл успешно
- Пользователи из других OU → диск Z: не отображается

---

## Вывод
- Централизованное управление доступами реализовано
- Принцип минимальных привилегий соблюден
- Подключение сетевых ресурсов автоматизировано через GPO

---

## Используемые инструменты

VirtualBox — виртуализация среды.

Windows Server 2022 — развертывание доменных служб Active Directory.

Windows 10 — рабочие станции, подключенные к домену.

Active Directory Domain Services (AD DS) — централизованное управление пользователями и группами.

Group Policy Management (GPO) — централизованная настройка параметров и автоматическое маппирование сетевых дисков.

NTFS — управление доступом на уровне файловой системы.

SMB — предоставление сетевого доступа к папкам. 


## Скриншоты
Все доказательства проделанной работы сохранены в папке screenshots.

## Автор проекта
Yunus Erbaev

GitHub: erbaevy
