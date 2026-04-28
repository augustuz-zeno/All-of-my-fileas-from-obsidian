[[0 Ustudy]]
### 1. Ввод данных (`read`)
Команда `read` используется для получения данных от пользователя.
- `read variable` — простейший ввод.
    
- `read -p "Prompt" variable` — ввод с текстом-приглашением.
    
- `read -sp "Prompt" variable` — скрытый ввод (для паролей).
---
### 2. Операторы сравнения чисел
В Bash для сравнения целых чисел используются специальные флаги:

| Флаг  | Описание         | Аналог |
| ----- | ---------------- | ------ |
| `-gt` | Greater than     | `>`    |
| `-ge` | Greater or equal | `>=`   |
| `-lt` | Less than        | `<`    |
| `-le` | Less or equal    | `<=`   |
| `-eq` | Equal            | `==`   |
| `-ne` | Not equal        | `!=`   |

Экспортировать в Таблицы
---
### 3. Условный оператор `if`
Синтаксис позволяет проверять условия и выполнять разные блоки кода.
**Простая проверка:**
```Bash
if [ condition ]
then
    # код, если истина
else
    # код, если ложь
fi
```
**Множественные условия (`elif`):** Для объединения условий используются:
- `-a` — логическое **И** (AND).
    
- `-o` — логическое **ИЛИ** (OR).
---
### 4. Работа с файловой системой
Код демонстрирует проверку существования объектов:
- `if [ -e $filename ]` — существует ли такой **файл**.
    
- `if [ -d $foldername ]` — существует ли такая **директория**.
Пример кода который писали на уроке:
```bash
!/bin/bash
# read -p "nima yaratmoqchisiz? (file/folder)" answer

# if [ $answer = "file" ]
# then 
#     read -p "fayl nomini kiriting: " filename
#     if [ -e  $filename ]
#     then 
#         echo "Tizimda bunday fayl topildi"
#     else 
#         touch $filename
#         echo " $filename nomli fayl muvaffaqiyatli yaratildi"
#     fi 
# else
#     read -p "Folder nomini kiriting:  " foldername
#     if [ -d $foldername ]
#     then  
#         echo "Tizimda bunday folder topildi"
#     else 
#         mkdir $foldername
#         echo " $foldername nomli folder muvaffaqiyatli yaratildi"
#     fi 
# fi 

read -p "yoshingizni kiriting"    yosh
if [ $yosh -lt 18 ]
then 
    echo "Voyaga yetmagan"
elif [ $yosh -ge 18 -a $yosh -le 30 ]
then 
    echo "Yoshsiz"
elif [ $yosh -gt 30 -a $yosh -le 50 ]
then 
    echo "O'rtacha yoshlisiz"
else 
    echo "Keksasiz"
fi
```