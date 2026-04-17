[[0 Ustudy]]
это было разобрано еще в [[3 Пользователи и привелигии]]
### /etc/passwd
Доступен всем (readable). Содержит информацию о пользователях.

**Формат:**
username:x:UID:GID:comment:home:shell

**Пример:**
john:x:1002:1002:John Doe:/home/john:/bin/bash

**Поля:**
- username — имя пользователя  
- x — пароль хранится в /etc/shadow  
- UID — user ID  
- GID — primary group ID  
- comment — описание (имя)  
- home — домашняя директория  
- shell — shell  

UID 0 = root (superuser)

---

### /etc/shadow
Файл "/etc/shadow" хранит хэши паролей и политику их действия. Доступен только root.
**Формат:**
username:hash:last_change:min:max:warn:inactive:expire:reserved

**Пример:**
root:$6$hash...:18781:0:99999:7:::

**Поля:**
- hash — хэш пароля
- Хэш пароля или спец-значение:
> - "$6$" — SHA-512
> - "$5$" — SHA-256
> - "$1$" — MD5
> - "!" — аккаунт заблокирован
> - "*" — вход запрещён
> Формат: $id$salt$hash
- last_change — дни с 01.01.1970  
- min — минимальный срок смены  
- max — максимальный срок  
- warn — предупреждение (За сколько дней предупреждать об истечении пароля)
- inactive — Через сколько дней после истечения пароль блокируется
- expire — дата истечения (Дата полной блокировки аккаунта (в днях с 1970)) 
- reserved
> - Зарезервировано на будущее
> - Не используется
> - Обычно пустое

---

Связанные моменты

- Используется вместе с "/etc/passwd"
- Контролируется через PAM ("pam_unix")
- Хэши нельзя расшифровать — только подбирать

---
### /etc/group
Содержит информацию о группах.

**Формат:**
group:x:GID:users

**Пример:**
bluetooth:x:117:kali

---
## UID и GID
- UID — уникальный ID пользователя  
- GID — ID группы  
- Дополнительные группы — /etc/group  

---
##  Управление аккаунтами

### Блокировка пароля
usermod -L username  
passwd -l username  

→ добавляет `!` к hash в /etc/shadow  

---
### Expire аккаунта
chage -E YYYY-MM-DD username  

Проверка:
chage -l username  

---
### Отключение входа (shell)
usermod -s /sbin/nologin username  

или:
usermod -s /bin/false username  

---

### Проверка статуса
passwd --status username  
chage -l username  
grep ^user /etc/passwd  

---
## sudo

### Выполнение с привилегиями
sudo command  

- требует пароль пользователя  
- кеш ~5 минут  

---
### Проверка прав
sudo -l  

---
### Root shell
sudo -i  

---
### Конфигурация
/etc/sudoers  

Пример:
root ALL=(ALL:ALL) ALL  
%sudo ALL=(ALL:ALL) ALL  

---
## Смена пользователя

### su
su  

→ вход как root (нужен пароль root)

su -l user  

su -l user -c "command"  

---
## Важно (Security)
- Проверять:
  - заблокирован ли пароль  
  - expire аккаунта  
  - shell (/sbin/nologin)  
- Ошибки конфигурации = уязвимости  
- UID 0 = полный доступ (не дублировать)
также [презентация](https://www.canva.com/design/DAG5HBraId0/F1YkQKkG2xVOXCmXo22_GQ/edit?utm_content=DAG5HBraId0&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton) и сам файл![[tarqatma_material_11_dars_Foydalanuvchi_va_guruhlar_bilan_ishlash.docx]]