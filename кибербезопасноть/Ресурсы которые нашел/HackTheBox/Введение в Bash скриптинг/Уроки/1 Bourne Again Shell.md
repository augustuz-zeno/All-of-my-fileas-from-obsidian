[[0 HTB Introduction to Bash Scripting]] 
[Bash](https://en.wikipedia.org/wiki/Bash_\(Unix_shell\)) — это язык сценариев, который мы используем для взаимодействия с ОС на базе Unix и передачи команд системе. С мая 2019 года Windows предоставляет [подсистему Windows для Linux (WSL)](https://docs.microsoft.com/en-us/windows/wsl/about), которая позволяет использовать `Bash` в среде Windows. Для эффективной работы важно в совершенстве владеть этим языком. Основное различие между скриптовыми и языками программирования заключается в том, что скрипты не требуют компиляции кода для выполнения.
Как пентестеры, мы должны уметь работать с любой операционной системой, будь то Windows или Unix. Эффективность во многом зависит от знания систем, особенно в области повышения привилегий. В системах на базе Unix крайне важно научиться использовать терминал, фильтровать данные и автоматизировать эти процессы. В крупных корпоративных сетях Unix нам придется иметь дело с огромными объемами данных. Мы должны уметь сортировать и фильтровать их, чтобы максимально быстро выявлять потенциальные бреши и информацию.
Также важно научиться комбинировать несколько команд и работать с промежуточными результатами. Именно здесь на помощь приходят скрипты, повышая нашу скорость и эффективность. Как и язык программирования, язык сценариев имеет схожую структуру, которую можно разделить на:
- `Ввод` и `Вывод`
    
- `Аргументы`, `Переменные` и `Массивы`
    
- `Условное выполнение`
    
- `Арифметика`
    
- `Циклы`
    
- `Операторы сравнения`
    
- `Функции`

Часто автоматизируют процессы, чтобы не повторять их постоянно или для обработки больших объемов информации. Как правило, скрипт не создает отдельный процесс, а выполняется интерпретатором (в данном случае `Bash`). Чтобы запустить скрипт, нужно указать интерпретатор и файл сценария. Это выглядит так:
#### Script Execution - Examples
```shell
august852@htb[/htb]$ bash script.sh <optional arguments>
august852@htb[/htb]$ sh script.sh <optional arguments>
august852@htb[/htb]$ ./script.sh <optional arguments>
```
Давайте рассмотрим пример скрипта для получения конкретных результатов. Если мы запустим его и укажем домен, мы увидим следующую информацию:
#### CIDR.sh
Фрагмент кода
```shell
august852@htb[/htb]$ ./CIDR.sh inlanefreight.com

Discovered IP address(es):
165.22.119.202

Additional options available:
    1) Identify the corresponding network range of target domain.
    2) Ping discovered hosts.
    3) All checks.
    *) Exit.

Select your option: 3

NetRange for 165.22.119.202:
NetRange:       165.22.0.0 - 165.22.255.255
CIDR:           165.22.0.0/16

Pinging host(s):
165.22.119.202 is up.

1 out of 1 hosts are up.
```
Теперь разберем этот скрипт построчно. В следующих разделах мы проанализируем все его части.
#### CIDR.sh
```Bash
#!/bin/bash

# Check for given arguments
if [ $# -eq 0 ]
then
    echo -e "You need to specify the target domain.\n"
    echo -e "Usage:"
    echo -e "\t$0 <domain>"
    exit 1
else
    domain=$1
fi

# Identify Network range for the specified IP address(es)
function network_range {
    for ip in $ipaddr
    do
        netrange=$(whois $ip | grep "NetRange\|CIDR" | tee -a CIDR.txt)
        cidr=$(whois $ip | grep "CIDR" | awk '{print $2}')
        cidr_ips=$(prips $cidr)
        echo -e "\nNetRange for $ip:"
        echo -e "$netrange"
    done
}

# Ping discovered IP address(es)
function ping_host {
    hosts_up=0
    hosts_total=0
    
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
    
    echo -e "\n$hosts_up out of $hosts_total hosts are up."
}

# Identify IP address of the specified domain
hosts=$(host $domain | grep "has address" | cut -d" " -f4 | tee discovered_hosts.txt)

echo -e "Discovered IP address:\n$hosts\n"
ipaddr=$(host $domain | grep "has address" | cut -d" " -f4 | tr "\n" " ")

# Available options
echo -e "Additional options available:"
echo -e "\t1) Identify the corresponding network range of target domain."
echo -e "\t2) Ping discovered hosts."
echo -e "\t3) All checks."
echo -e "\t*) Exit.\n"

read -p "Select your option: " opt

case $opt in
    "1") network_range ;;
    "2") ping_host ;;
    "3") network_range && ping_host ;;
    "*") exit 0 ;;
esac
```
---
Как мы видим, скрипт разделен комментариями на несколько логических частей:

1. Проверка аргументов
    
2. Определение сетевого диапазона для указанных IP
    
3. Пинг обнаруженных IP-адресов
    
4. Определение IP-адресов указанного домена
    
5. Доступные опции
---
#### 1. Проверка аргументов
В первой части используется условие if-else, которое проверяет, указан ли домен целевой компании.

---
#### 2. Определение сетевого диапазона для указанных IP
Здесь создана функция, которая выполняет запрос "whois" для каждого IP, отображает строку с зарезервированным диапазоном сети и сохраняет её в CIDR.txt.

---
#### 3. Пинг обнаруженных IP-адресов
Эта функция проверяет доступность найденных хостов. С помощью цикла For мы пингуем каждый IP в диапазоне и подсчитываем результаты.

---
#### 4. Определение IP-адресов указанного домена
Первым шагом в работе скрипта мы определяем IPv4-адрес, который возвращает домен.

---
#### 5. Доступные опции
Затем мы выбираем, какие функции использовать, чтобы узнать больше об инфраструктуре.