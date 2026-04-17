[[0 HTB Introduction to Bash Scripting]]
В Bash у нас семь разных `arithmetic operators`Мы можем работать с нами. Они используются для выполнения различных математических операций или для изменения определенных целых чисел.
#### Операторы арифметики

| **Оператор** | **Описание**                       |
| ------------ | ---------------------------------- |
| `+`          | Дополнение                         |
| `-`          | Вычитание                          |
| `*`          | Умножение                          |
| `/`          | Дивизион                           |
| `%`          | Модул                              |
| `variable++` | Увеличить значение переменной на 1 |
| `variable--` | Уменьшить значение переменной на 1 |
Мы можем обобщить все эти операторы в небольшом сценарии:
#### Arithmetic.sh
```bash
#!/bin/bash

increase=1
decrease=1

echo "Addition: 10 + 10 = $((10 + 10))"
echo "Subtraction: 10 - 10 = $((10 - 10))"
echo "Multiplication: 10 * 10 = $((10 * 10))"
echo "Division: 10 / 10 = $((10 / 10))"
echo "Modulus: 10 % 4 = $((10 % 4))"

((increase++))
echo "Increase Variable: $increase"

((decrease--))
echo "Decrease Variable: $decrease"
```
Выход этого сценария выглядит так:
#### Arithmetic.sh - Execution
```shell
behzod789@htb[/htb]$ ./Arithmetic.sh

Addition: 10 + 10 = 20
Subtraction: 10 - 10 = 0
Multiplication: 10 * 10 = 100
Division: 10 / 10 = 1
Modulus: 10 % 4 = 2
Increase Variable: 2
Decrease Variable: 0
```
---
Мы также можем рассчитать длину переменной. С помощью этой функции `${#variable}`, каждый персонаж засчитывается, и мы получаем общее количество символов в переменной.
#### VarLength.sh
```bash
#!/bin/bash

htb="HackTheBox"

echo ${#htb}
```
#### VarLength.sh
```bash
behzod789@htb[/htb]$ ./VarLength.sh

10
```
---
Если мы посмотрим на наш `CIDR.sh`Сценарий, мы увидим, что мы использовали `increase`и `decrease`Операторы несколько раз. Это гарантирует, что цикл времени, который мы обсудим позже, запускает и пингует хосты, в то время как переменная "`stat`" имеет значение `1`. Если команда ping заканчивается кодом `0`(успешно), мы получаем сообщение, что `host is up`и "`stat`" переменная, а также переменные "`hosts_up`" и "`hosts_total`" изменятся.
#### CIDR.sh
```bash
<SNIP>
    echo -e "\nPinging host(s):"
    for host in $cidr_ips
    do
        stat=1
        while [ $stat -eq 1 ]
        do
            ping -c 2 $host > /dev/null 2>&1
            if [ $? -eq 0 ]
            then
                echo "$host is up."
                ((stat--))
                ((hosts_up++))
                ((hosts_total++))
            else
                echo "$host is down."
                ((stat--))
                ((hosts_total++))
            fi
        done
    done
<SNIP>
```