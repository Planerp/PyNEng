## Модуль netmiko

Netmiko это модуль, который позволяет упростить использование paramiko для сетевых устройств.

Грубо говоря, netmiko это такая "обертка" для paramiko.

Сначала Netmiko нужно установить:
```
pip install netmiko
```

Посмотрим на пример, который мы использовали ранее, но теперь с использованием netmiko:
```python
from netmiko import ConnectHandler
import getpass
import sys


COMMAND = sys.argv[1]
USER = raw_input("Username: ")
PASSWORD = getpass.getpass()
ENABLE_PASS = getpass.getpass(prompt='Enter enable password: ')

DEVICES_IP = ['192.168.100.1','192.168.100.2','192.168.100.3']

for IP in DEVICES_IP:
    print "Connection to device %s" % IP
    DEVICE_PARAMS = {'device_type': 'cisco_ios',
                     'ip': IP,
                     'username':USER,
                     'password':PASSWORD,
                     'secret':ENABLE_PASS }

    ssh = ConnectHandler(**DEVICE_PARAMS)
    ssh.enable()
    
    result = ssh.send_command(COMMAND)
    print result
```

Посмотрите насколько проще выглядит этот пример с netmiko.

Разберемся с содержимым скрипта:
* DEVICE_PARAMS - это словарь, в котором мы определяем параметры устройства
 * device_type - это предопределенные значения, которые понимает netmiko
    * в данном случае, так как мы подключаемся к устройству с Cisco IOS, мы используем значение 'cisco_ios'
* ```ssh = ConnectHandler(**DEVICE_PARAMS)``` - устанавливаем соединение с устройством, на основе параметров, которые мы передали в словаре
* ```ssh.enable()``` - переходим в режим enable
 * пароль передается автоматически
 * используется значение ключа secret, которое мы указали в словаре DEVICE_PARAMS
* ```result = ssh.send_command(COMMAND)``` - отправляем команду и получаем вывод

Возможно, вы обратили внимание, что в этом примере мы не передавали команду terminal length. Мы не делали это, так как netmiko настраивает это сам по умолчанию.


Так выглядит результат выполнения скрипта:
```
Username: cisco
Password:
Enter enable password:
Connection to device 192.168.100.1
Interface              IP-Address      OK? Method Status                Protocol
FastEthernet0/0        192.168.100.1   YES NVRAM  up                    up
FastEthernet0/1        unassigned      YES NVRAM  up                    up
FastEthernet0/1.10     10.1.10.1       YES manual up                    up
FastEthernet0/1.20     10.1.20.1       YES manual up                    up
FastEthernet0/1.30     10.1.30.1       YES manual up                    up
FastEthernet0/1.40     10.1.40.1       YES manual up                    up
FastEthernet0/1.50     10.1.50.1       YES manual up                    up
FastEthernet0/1.60     10.1.60.1       YES manual up                    up
FastEthernet0/1.70     10.1.70.1       YES manual up                    up
Connection to device 192.168.100.2
Interface              IP-Address      OK? Method Status                Protocol
FastEthernet0/0        192.168.100.2   YES NVRAM  up                    up
FastEthernet0/1        unassigned      YES NVRAM  up                    up
FastEthernet0/1.10     10.2.10.1       YES manual up                    up
FastEthernet0/1.20     10.2.20.1       YES manual up                    up
FastEthernet0/1.30     10.2.30.1       YES manual up                    up
FastEthernet0/1.40     10.2.40.1       YES manual up                    up
FastEthernet0/1.50     10.2.50.1       YES manual up                    up
FastEthernet0/1.60     10.2.60.1       YES manual up                    up
FastEthernet0/1.70     10.2.70.1       YES manual up                    up
Connection to device 192.168.100.3
Interface              IP-Address      OK? Method Status                Protocol
FastEthernet0/0        192.168.100.3   YES NVRAM  up                    up
FastEthernet0/1        unassigned      YES NVRAM  up                    up
FastEthernet0/1.10     10.3.10.1       YES manual up                    up
FastEthernet0/1.20     10.3.20.1       YES manual up                    up
FastEthernet0/1.30     10.3.30.1       YES manual up                    up
FastEthernet0/1.40     10.3.40.1       YES manual up                    up
FastEthernet0/1.50     10.3.50.1       YES manual up                    up
FastEthernet0/1.60     10.3.60.1       YES manual up                    up
FastEthernet0/1.70     10.3.70.1       YES manual up                    up
```

В выводе нет никаких лишних приглашений, только вывод команды sh ip int br.

Так как netmiko наиболее удобный модуль для подключения к сетевому оборудования, мы разберемся с ним подробней.