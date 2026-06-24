[[0 Ustudy]]
### Управление правилами брандмауэра (netsh advfirewall)

Команда `netsh advfirewall firewall` используется для настройки правил фильтрации трафика в Windows.

**Базовые параметры правил:**

- **dir (direction):** Направление трафика.
    
    - `in`: Входящий трафик (на устройство).
        
    - `out`: Исходящий трафик (с устройства).
        
- **action:** Действие над пакетом.
    
    - `block`: Блокировка пакета.
        
    - `allow`: Разрешение прохождения пакета.
        
- **protocol:** Тип протокола (icmp, tcp, udp).
    
- **remoteport / localport:** Указание портов для фильтрации.
    
- **enable=yes:** Активация правила сразу после создания.
    

**Примеры команд:**

- **Блокировка ICMP (пинг):** `netsh advfirewall firewall add rule name="ICMP Block" dir=in action=block protocol=icmp`
    
- **Блокировка исходящего HTTPS (порт 443):** `netsh advfirewall firewall add rule name="BLOCK_HTTPS_OUT" dir=out action=block protocol=tcp remoteport=443 enable=yes`
    
- **Блокировка входящего HTTPS (порт 443):** `netsh advfirewall firewall add rule name="BLOCK_HTTPS_IN" dir=in action=block protocol=tcp localport=443 enable=yes`
    

### Полное отключение интернет-соединения

Для блокировки всех сетевых соединений (входящих и исходящих) через `netsh`:

1. **Блокировка всего входящего трафика:** `netsh advfirewall set allprofiles firewallpolicy blockinbound,blockoutbound`
    

_Примечание: Эта команда переводит профили брандмауэра в режим жесткой блокировки. Для восстановления доступа по умолчанию используйте `set allprofiles firewallpolicy allowinbound,allowoutbound`._

### Управление процессами и службами

Для диагностики и управления запущенными программами и системными службами используются следующие инструменты:

**Просмотр списков:**

- `tasklist`: Выводит список всех запущенных процессов с идентификаторами (PID) и потреблением памяти.
    
- `pslist`: (Требует установки Sysinternals) Аналог `tasklist`, но с более детальной информацией о потоках и времени работы процесса.
    

**Управление службами (Service Control):**

- `sc`: Командная строка для связи с Service Control Manager.
    
    - Пример получения статуса службы: `sc query <имя_службы>`
        
- `net start`: Вывод списка запущенных служб или запуск конкретной службы:
    
    - `net start <имя_службы>`
        
- `net stop`: Остановка службы:
    
    - `net stop <имя_службы>`
        

**Дополнительно:** Для удаления правила брандмауэра используйте: `netsh advfirewall firewall delete rule name="ИМЯ_ПРАВИЛА"`