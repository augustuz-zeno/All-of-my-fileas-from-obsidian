[[0 Ustudy]]
# Решение CTF:
## Есть zip файл chall.zip
### Тут есть два пути для распаковки архива: 
### Метод 1: Брутфорс (если пароль неизвестен) Извлекаем хеш пароля и перебираем его по словарю `rockyou.txt`. 
```bash 
zip2john chall.zip > hash.txt 
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```
![[2-17.png]]
Но процесс очень долгий поэтому лучше использовать 2 метод, если пароль уже известен.
### Метод 2: Распаковка (пароль `iut`)
Если пароль уже известен, используем `7z`:
```Bash 
7z x chall.zip
# Ввести пароль: iut и потмо нажать на Enter
```
![[3-17.png]]
После перехода в папку `chall` изучаем файл `README_CHALLENGE.txt`. Описание флагов:

- **Flag 1:** `!ylluferac ti deaR`
    
- **Flag 2:** `Pass2Link`
    
- **Flag 3:** `WH questions ???` (Формат: `CHILLCHAIN{WHEN_WHO}`)
    
- **Flag 4:** `Backup Logs`
### Флаг №1
Подсказка и сам флаг в файле `home/chillchain/wow.txt` записаны зеркально. **Команда для расшифровки:**
```Bash
echo "}!!!detacol_galf_ysae{NIAHCLLIHC" | rev
```
**Результат:** `CHILLCHAIN{easy_flag_located!!!}`
![[4-17.png]]
### Флаг №2
Необходимо найти пароль пользователя из файла `chall/etc/shadow`.
![[5-17.png]]
1. **Взлом хеша:**    
```Bash
john --wordlist=/usr/share/wordlists/rockyou.txt shadow
# Или для просмотра найденного:
john --show shadow
```
![[6-17.png]]
2. **Получение флага:** Пароль `september`. Вводим его на сайте и получаем вот такой текст:
> [!NOTE]
> Y0U cr4ck3d TH3 p4:
> here is your flag: Q0hJTExDSEFJTntjcjRjazNkX3RoZV9wYXNzISEhfQ==
> ))

Отсюда нужно взять шифр Base64.
```Bash
echo "Q0hJTExDSEFJTntjcjRjazNkX3RoZV9wYXNzISEhfQ==" | base64 -d
```
**Результат:** `CHILLCHAIN{cr4ck3d_the_pass!!!}`
![[7-17.png]]
### Флаг №3
Флаг находится в логах `chall/var/log/auth.log`. 
![[8-17.png]]
Нужно найти время и имя пользователя с помощью вот этой команды:
```Bash
grep "Accepted" auth.log | grep "chillchain" | awk '{print "CHILLCHAIN{"$1" "$2" "$3"_"$9"}"}'
```
![[9-17.png]]
**Результат:** `CHILLCHAIN{Sep 10 09:15:42_chillchain}`
### Флаг №4
Флаг разбит на 4 части в директории `chall/var/log/backup_logs` (папки part A-D).
1. **Сбор фрагментов:**
```Bash
cat part-A.log part-B.log part-C.log part-D.log
```
Получается вот такой вывод:
> [!NOTE]
> noise line - %ignore% - maybe do not ignore
> ---BEGIN---
> Q0hJTExDSEFJ
> ---END---
> random header - maybe not random
> Tntsb2dfaHVu
> trailing comment
> dF9tYXN0ZXJl
> ZH0=
> end of fragments

![[10-17.png]]
2. **Декодирование:**
```Bash
echo "Q0hJTExDSEFJTntsb2dfaHVudF9tYXN0ZXJlZH0=" | base64 -d
```
![[11-17.png]]
**Результат:** `CHILLCHAIN{log_hunt_mastered}`. Это последний флаг и конец CTF.