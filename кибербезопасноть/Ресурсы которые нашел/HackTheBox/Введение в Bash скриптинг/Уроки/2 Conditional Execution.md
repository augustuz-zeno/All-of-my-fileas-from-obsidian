[[0 HTB Introduction to Bash Scripting]]  
Условное выполнение позволяет нам управлять потоком нашего скрипта, достигая различных условий. Эта функция является одним из основных компонентов. В противном случае мы могли бы только выполнять одну команду за другой.  
При определении различных условий мы указываем, какие функции или участки кода должны выполняться для конкретного значения. Если достигается определённое условие, выполняется только код для этого условия, а остальные пропускаются. Как только выполнение участка кода завершено, следующие команды будут выполнены вне условного блока. Давайте снова рассмотрим первую часть скрипта и проанализируем её.
#### Script.sh
```bash
#!/bin/bash  
  
# Check for given argument  
if [ $# -eq 0 ]  
then  
    echo -e "You need to specify the target domain.\n"  
    echo -e "Usage:"  
    echo -e "\t$0 <domain>"  
    exit 1  
else  
    domain=$1  
fi  
  
<SNIP>
```
Вкратце, этот участок кода работает со следующими компонентами:
- `#!/bin/bash` - Shebang.
- `if-else-fi` - Условное выполнение.
- `echo` - Выводит определённый текст.
- `$#` / `$0` / `$1` - Специальные переменные.
- `domain` - Переменные.
Условия условного выполнения могут быть определены с использованием переменных (`$#`, `$0`, `$1`, `domain`), значений (`0`) и строк, как мы увидим в следующих примерах. Эти значения сравниваются с помощью `операторов сравнения` (`-eq`), которые мы рассмотрим в следующем разделе.
---
## Shebang
Строка shebang всегда находится в начале каждого скрипта и всегда начинается с "`#!`". Эта строка содержит путь к указанному интерпретатору (`/bin/bash`), с помощью которого выполняется скрипт. Мы также можем использовать Shebang для указания других интерпретаторов, таких как Python, Perl и другие.
```python
#!/usr/bin/env python
```

```perl
#!/usr/bin/env perl
```
---
## If-Else-Fi
Одной из самых фундаментальных задач программирования является проверка различных условий для работы с ними. Проверка условий обычно имеет две разные формы в языках программирования и сценариев: `if-else condition` и `case statements`. В псевдокоде условие if означает следующее:
#### Pseudo-Code
```bash
if [ the number of given arguments equals 0 ]  
then  
    Print: "You need to specify the target domain."  
    Print: "<empty line>"  
    Print: "Usage:"  
    Print: "   <name of the script> <domain>"  
    Exit the script with an error  
else  
    The "domain" variable serves as the alias for the given argument   
finish the if-condition
```
По умолчанию условие `If-Else` может содержать только один "`If`", как показано в следующем примере.
#### If-Only.sh
```bash
#!/bin/bash  
  
value=$1  
  
if [ $value -gt "10" ]  
then  
        echo "Given argument is greater than 10."  
fi
```
#### If-Only.sh - Execution
```shell
august852@htb[/htb]$ bash if-only.sh 5  
  
august852@htb[/htb]$ bash if-only.sh 12  
  
Given argument is greater than 10.
```
---
При добавлении `Elif` или `Else` мы добавляем альтернативы для обработки конкретных значений или состояний. Если определённое значение не подходит под первый случай, оно будет обработано другими.
#### If-Elif-Else.sh
```bash
#!/bin/bash  
  
value=$1  
  
if [ $value -gt "10" ]  
then  
    echo "Given argument is greater than 10."  
elif [ $value -lt "10" ]  
then  
    echo "Given argument is less than 10."  
else  
    echo "Given argument is not a number."  
fi
```
#### If-Elif-Else.sh - Execution
```shell
august852@htb[/htb]$ bash if-elif-else.sh 5  
  
Given argument is less than 10.  
  
august852@htb[/htb]$ bash if-elif-else.sh 12  
  
Given argument is greater than 10.  
  
august852@htb[/htb]$ bash if-elif-else.sh HTB  
  
if-elif-else.sh: line 5: [: HTB: integer expression expected  
if-elif-else.sh: line 8: [: HTB: integer expression expected  
Given argument is not a number.
```
---
Мы можем расширить наш скрипт и задать несколько условий. Это может выглядеть примерно так:
#### Several Conditions - Script.sh
```bash
#!/bin/bash  
  
# Check for given argument  
if [ $# -eq 0 ]  
then  
    echo -e "You need to specify the target domain.\n"  
    echo -e "Usage:"  
    echo -e "\t$0 <domain>"  
    exit 1  
elif [ $# -eq 1 ]  
then  
    domain=$1  
else  
    echo -e "Too many arguments given."  
    exit 1  
fi  
  
<SNIP>
```
---
Здесь мы определяем ещё одно условие (`elif [<condition>];then`), которое выводит строку (`echo -e "..."`), сообщающую нам, что было передано более одного аргумента, и завершает программу с ошибкой (`exit 1`).

---
## Exercise Script
```bash
#!/bin/bash  
# Count number of characters in a variable:  
#     echo $variable | wc -m  
  
# Variable to encode  
var="nef892na9s1p9asn2aJs71nIsm"  
  
for counter in {1..40}  
do  
        var=$(echo $var | base64)  
done
```
