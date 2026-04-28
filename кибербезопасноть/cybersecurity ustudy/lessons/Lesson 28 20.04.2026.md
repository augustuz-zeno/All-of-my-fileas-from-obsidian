[[0 Ustudy]]
Сегодня урока не было, учитель дал задание чтобы мы выполнили его до следующего урока. Задание:
> [!INFO]
> Bash script tuzing:
> Foydalanuvchi sudo orqali skripni ishlatishi kerak.
> - 1 Tizimda yangi foydalanuvchi qoshish.
> - 2 Tizimdagi user larni list qilish.
> - 3 Tizimdagi userni oçhirish
> - 4 Ekranni tozalash.
> - 5 Chiqish.

Вот код:
```bash
#!/bin/bash

if [ "$EUID" -ne 0 ]; then
  echo "Use sudo."
  exit 1
fi

while true
do
  echo "1) Add user 2) User list 3) Del user 4) Clear 5) Exit"
  read -p "Choose option 1..5: " x
  if [ "$x" = "1" ]; then
    read -p "Name of new user: " user
    useradd "$user"
    passwd "$user"
  elif [ "$x" = "2" ]; then
    echo "Users with login shell:"
    grep -vE 'nologin|false' /etc/passwd | cut -d: -f1
  elif [ "$x" = "3" ]; then
    read -p "Type username to deluser: " user
    userdel -r "$user"
  elif [ "$x" = "4" ]; then
    clear
  elif [ "$x" = "5" ]; then
    echo "Exit..."
    exit 0
  else
    echo "Error, choose 1..5"
  fi
done
```
Объяснение: