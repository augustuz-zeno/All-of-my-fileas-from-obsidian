[[0 Ustudy]]
Python скриптинг позже составлю полноценный конспект
```Python
#!/usr/bin/python3


import os
from random import random
from random import randint    

# ism = str(input("What is your name? "))
            
# print(f"Salom {ism}")
# print(ism)



# yosh =int(input("Yoshingni kiriti! "))

# print(f"Sening yoshing {yosh}da ekan!")

# filename = str(input("Fayl nomini kiriting: "))

# with open(filename, 'r') as data:
#     print(data.read())


# data = str(input("Qaysi faylga yozmoqchisiz? "))
# xat = str(input("Qanday malumot yozmoqchisiz? "))

# with open(data, 'w') as u16:
#     u16.write(xat)

# os.system("whoami")

# yosh = int(input("Yoshingizni kiriting: "))
# if yosh < 18:
#     print("Siz hali voyaga yetmagansiz!")
# elif yosh >= 18 and yosh < 30:
#     print("O'rta yosh")
# else:
#     print("Siz voyaga yetgansiz!")



# for i in range(1, 100):
#     print(i)
    
#i = int(input("son kiriting: "))
# while i <= 10:
#     # print(i)
    # i += 1 


qoldiq=15


while qoldiq > 0:
    son=int(input("Son kiriting: "))
    haqiqiy_son=randint(1, 10)
    if son == haqiqiy_son:
        print("US{Siz_flagni_topdingiz}")
        break
    else:
      print("Notogri son tanladingiz")
      qoldiq -= 1
      print(f"Qolgan urinishlar soni: {qoldiq}")
```
не чего то нового, для меня не более чем повтор.