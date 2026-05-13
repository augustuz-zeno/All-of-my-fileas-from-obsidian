[[0 HTB Introduction to Bash Scripting]]
# Задание из [[2 Conditional Execution]]
### Create an "If-Else" condition in the "For"-Loop of the "Exercise Script" that prints you the number of characters of the 35th generated value of the variable "var". Submit the number as the answer.
```bash
#!/bin/bash
# Count number of characters in a variable:
#     echo $variable | wc -m

# Variable to encode
var="nef892na9s1p9asn2aJs71nIsm"

for counter in {1..35}
do
        var=$(echo $var | base64)
done
echo $var | wc -m
```
Вот так вот выглядит ответ на этот вопрос, данный код просто кодирует результат 35 раз и после 35 раза подсчитывает количество строк, ответ будет: **1197735**.
# Задание из [[3 Arguments, Variables, and Arrays]]
### Submit the echo statement that would print "www2.inlanefreight.com" when running the last "Arrays.sh" script.
```bash
#!/bin/bash

domains=("www.inlanefreight.com ftp.inlanefreight.com vpn.inlanefreight.com" www2.inlanefreight.com)
echo ${domains[1]}
```

```bash
behzod789@htb[/htb]$ ./Arrays.sh

www2.inlanefreight.com
```
В массиве `domains` **два элемента**, а не четыре:
    - `domains[0]` = `"www.inlanefreight.com ftp.inlanefreight.com vpn.inlanefreight.com"` (одна строка, потому что в кавычках)
    - `domains[1]` = `www2.inlanefreight.com`
Bash воспринимает всё в кавычках как **один элемент массива**, даже если внутри есть пробелы. Поэтому, чтобы получить `www2.inlanefreight.com`, нужно обратиться ко **второму элементу** → `${domains[1]}`
# Задание из [[4 Comparison Operators]]
### Create an "If-Else" condition in the "For"-Loop that checks if the variable named "var" contains the contents of the variable named "value". Additionally, the variable "var" must contain more than 113,450 characters. If these conditions are met, the script must then print the last 20 characters of the variable "var". Submit these last 20 characters as the answer.
```bash
#!/bin/bash
var1="8dm7KsjU28B7v621Jls" 
value="ERmFRMVZ0U2paTlJYTkxDZz09Cg" 
for i in {1..40} 
do 
    var1=$(echo $var1 | base64) 
    var2=$(echo $var1 | wc -c) 
    if [[ "$var1" == *"$value"* && $var2 -gt 113450 ]];
    then 
    echo $var1 | tail -c 20
    break
    fi 
done
```
### Разбор логики
1. **Инициализация**:
    - `var1`: Исходная строка для преобразования.
        
    - `value`: Целевое значение, с которым будет идти сравнение.
2. **Цикл `for`**:
    - Выполняется **40 итераций**. На каждом шаге скрипт пытается обновить переменную `var1`.
3. **Условие `if`**:
    - Проверяет, совпадает ли текущая итерация кодировки с `value`.
        
    - Проверяет длину строки (`wc -c`): она должна быть больше **113 450** символов.
4. **Результат**:
    - Если оба условия верны, команда `tail -c 20` выводит последние **20 символов** получившейся строки.
И получается ответ: `2paTlJYTkxDZz09Cg==`
# Задание из [[7 Flow Control - Loops]]
### Create a "For" loop that encodes the variable "var" 28 times in "base64". The number of characters in the 28th hash is the value that must be assigned to the "salt" variable.
```bash
#!/bin/bash
function decrypt {
    MzSaas7k=$(echo $hash | sed 's/988sn1/83unasa/g')
    Mzns7293sk=$(echo $MzSaas7k | sed 's/4d298d/9999/g')
    MzSaas7k=$(echo $Mzns7293sk | sed 's/3i8dqos82/873h4d/g')
    Mzns7293sk=$(echo $MzSaas7k | sed 's/4n9Ls/20X/g')
    MzSaas7k=$(echo $Mzns7293sk | sed 's/912oijs01/i7gg/g')
    Mzns7293sk=$(echo $MzSaas7k | sed 's/k32jx0aa/n391s/g')
    MzSaas7k=$(echo $Mzns7293sk | sed 's/nI72n/YzF1/g')
    Mzns7293sk=$(echo $MzSaas7k | sed 's/82ns71n/2d49/g')
    MzSaas7k=$(echo $Mzns7293sk | sed 's/JGcms1a/zIm12/g')
    Mzns7293sk=$(echo $MzSaas7k | sed 's/MS9/4SIs/g')
    MzSaas7k=$(echo $Mzns7293sk | sed 's/Ymxj00Ims/Uso18/g')
    Mzns7293sk=$(echo $MzSaas7k | sed 's/sSi8Lm/Mit/g')
    MzSaas7k=$(echo $Mzns7293sk | sed 's/9su2n/43n92ka/g')
    Mzns7293sk=$(echo $MzSaas7k | sed 's/ggf3iunds/dn3i8/g')
    MzSaas7k=$(echo $Mzns7293sk | sed 's/uBz/TT0K/g')

    flag=$(echo $MzSaas7k | base64 -d | openssl enc -aes-128-cbc -a -d -salt -pass pass:$salt)
}

var="9M"
salt=""
hash="VTJGc2RHVmtYMTl2ZnYyNTdUeERVRnBtQWVGNmFWWVUySG1wTXNmRi9rQT0K"

for i in {1..28}
do
    var=$(echo $var | base64)
    
    if [[ $i == 28 ]]
    then
        salt=$(echo $var | wc -c)
    fi    
done

if [[ ! -z "$salt" ]]
then
    decrypt
    echo $flag
else
    exit 1
fi
```

### 1. Подготовка соли ($salt)
Цикл `for` 28 раз кодирует строку `9M` в формат **Base64**.
- На каждой итерации строка становится длиннее.
    
- После 28-й итерации переменная `$salt` получает значение, равное **количеству символов** в итоговой строке (через `wc -c`).
### 2. Функция `decrypt`
Эта функция обрабатывает переменную `$hash`:
- **Замены (sed):** Выполняется 15 последовательных замен одних подстрок на другие. Это примитивный способ обфускации (запутывания) данных. Переменные `MzSaas7k` и `Mzns7293sk` просто передают результат друг другу по цепочке.
    
- **Декодирование:** 1. Итоговая строка после замен декодируется из **Base64**. 2. Затем она расшифровывается через **OpenSSL** с использованием алгоритма **AES-128-CBC**. В качестве пароля используется вычисленная ранее `$salt`.
### 3. Логика запуска
- Скрипт проверяет, удалось ли вычислить `$salt`.
    
- Если да, вызывается `decrypt` и выводится расшифрованный результат (`$flag`).
    
- Если нет, скрипт завершается с ошибкой.