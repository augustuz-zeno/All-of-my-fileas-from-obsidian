[[0 Ustudy]]
```bash
#!/bin/bash


# echo "Yoshingizni kiriting!"
# read yosh
# echo "Ismingizni kiriting!"
# read ism
# echo "Salom, $ism, sizning yoshingiz $yosh da"



# read -p "Ismingizni kiriting!  " name
# read -sp "Parolingizni kiriting!  " parol
# echo ""
# echo $name $parol

# read qator
# echo $qator

# read qator
# echo $qator

# read qator
# echo $qator

# read qator
# echo $qator


# read -p "yoshing nechada? " yosh
# if [ $yosh -lt 18  ]
# then 
    # echo "Voyaga yetmagan"
# else
    # echo "Voyaga yetgan"
# fi
# 
# gt -- greater than -- dan katta (>)
# ge -- greater or equal -- katta yoki teng (>=)
# lt -- less then -- dan  kichkina (<)
# le -- less or equal -- kichkina yoki teng (<=)
# eq -- equal -- teng (==)
# ne -- not equal -- teng emas (!=) 
#  


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