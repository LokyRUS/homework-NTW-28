# `Подключение к оборудованию, базовая настройка` 
# Исполнитель: Смирнов Кирилл NTW-28
### Цель задания

1. Создать сетевую топологию.
2. Настроить сетевые устройства.


# Задание 1. 

Опишите возможные типы соединения с сетевыми устройствами: маршрутизаторы, коммутаторы, концентраторы. 

*Приведите тип подключения и опишите их характеристики.*

# Ответ:
К коммутатору и маршрутизатору, для получения доступа к управлению ими, можно подключиться как проводным путём через консольный порт, так и через удалённое подключение, последнее возможно произвести только после настройки ssh на конкретном оборудовании.

К хабу подключиться нет возможности, так как он не имеет внутреннего интерфейса для работы с ним, примитивное неуправляемое устройство, которое ретранслирует входящий сигнал в один из портов на все остальные порты, используя метод затопления.

# Задание 2.

Опишите отличия в подключении  к устройствам для их настройки через порты AUX, Console? 

*Приведите ответ в свободной форме.*

# Ответ

Для подключения к AUX можно использовать телефонное коммутируемое соединение. Для этого нужен модем, который соединён с портом маршрутизатора AUX или локально в качестве консольного порта при прямом подключении к компьютеру, который выполняет программу эмуляции терминала. 

Для подключения к консольному порту , потребуется один из консольных кабелей: 
-  rj45  - FTDI rs232 - классический 
# ![image1](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/1.PNG)

- rj45  - FTDI rs232 - c USB конвектором
# ![image2](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/2.PNG)

И эмулятор терминала, например, Putty. 
Важно : со стороны терминала, при подключении через консольный кабель, потребуются
дополнительная настройка параметров в соответствии с тем, что указано в документации оборудования.
# Задание 3.

Опишите плюсы и минусы подключения к устройствам для работы с ними по протоколам Telnet, ssh, http, https.
Какие методы безопасности могут быть предусмотрены для усиления защиты?  

*Приведите ответ в свободной форме.*

#  Ответ
`Telnet`
- `-` небезопасен, так как передает данные в виде обычного текста;
- `-` Не отображает графику.
- `+` удобен в использовании в рамказ закрытой локальной сети.

`ssh`
- `-` требуется больше настроек для работв;
- `+` используется TLS криптографический протокол,для обеспечения безопасной передачи данных на транспортном уровне;
- `+` Можно использовать не стандартный порт, повышает безопасность.

`http`
- `-` небезопасен,передача данных в виде обычного текста;
- `-` подключение будет отсутствовать, если на оборудовании не предусмотрен веб-интерфейс для отладки. 
- `+` - можно использовать и используется в локальном подключении к оборудованию, в рамках локальной сети.

`https`

- `-` - в отличие от сетевого протокола ssh, данный протокол работает на прикладном уровне и не сможет нам обеспечить подключение, если оборудование не имеет веб-интерфейс. 
- `+` - единственная разница, в сравнении с вышеуказанным протоколом, это возможность шифровать данные, при осуществлении передачи.

### Задание 4. Лабораторная работа "Подключение и базовая настройка сетевого оборудования"

Выполните задание по шагам:

1. В Cisco Packet Tracer cоздайте логическую топологию, состоящую из:
- компьютера для управления,
- коммутатора,
- маршрутизатора.
2. Подключите устройства последовательно.
3. Настройте устройства для сетевого взаимодействия.
4. Проверьте доступность компьютера и маршрутизатора при помощи ICMP запросов. 
5. На коммутаторе настройте протокол Telnet. Проверьте, что удаленное подключение возможно и работает успешно. Пароль должен быть зашифрован!
6. На маршрутизаторе настройте протокол SSH. Проверьте, что удаленное подключение возможно и работает успешно. Пароль должен быть зашифрован!

*Приложите настроенные логины и пароли к решению, приведите ответ в виде pkt-файла*

# Ответ

`Коммутатор`

### Присвоение виртуальному локальному интерфейсу ip адреса, для дальнейшего удаленного взаимодейсвия с ним.
```
Switch>enable 
Switch#configure terminal 
Switch(config)#interface vlan 1
Switch(config-if)#ip address 192.168.0.100 255.255.255.0
Switch(config-if)#no shutdown 
Switch(config-if)#
%LINK-5-CHANGED: Interface Vlan1, changed state to up
%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up
Switch(config-if)#exit 
Switch(config)#exit
Switch#wr
Building configuration...
[OK]
```

`Маршрутизатор`

### Присвоение интерфейсу ip адреса, для дальнейшего удаленного взаимодейсвия с ним.
```
Router>enable
Router#
Router#configure terminal
Router(config)#interface GigabitEthernet0/0/0
Router(config-if)#ip address 192.168.0.1 255.255.255.0
Router(config-if)#no shutdown 
Router(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/0, changed state to up
%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/0, changed state to up
Router(config-if)#exit
Router(config)#exit
Router#
%SYS-5-CONFIG_I: Configured from console by console
wr
Building configuration...
[OK]
Router#
```
### Нахначим нажешу хосту ip адресс
# ![image3](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/3.PNG)

## Получившаяся топология
# ![image4](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/4.PNG)

## Пингуем коммутатор и маршрутизатор

# ![image5](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/5.PNG)


# `Telnet` - настройка на коммутаторе 
```
Switch>enable 
Switch#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#use
Switch(config)#username admin secret admin
Switch(config)#line vty 0 15
Switch(config-line)#login local 
Switch(config-line)#exit
Switch(config)#enable secret Cisco	
Switch(config)#exit
Switch#
Switch#wr
Building configuration...
[OK]
Switch#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#do sh run | i user
username admin secret 5 $1$mERr$vTbHul1N28cEp8lkLqr0f/ # Зашифрованный пароль
Switch(config)#
```
`Проверка работоспособности`
# ![image6](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/6.PNG)

# `SSH` - настройка на маршрутизаторе
```
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#hostname R1
R1(config)#username admin secret admin
R1(config)#line vty 0 15
R1(config-line)#login local 
R1(config-line)#exit
R1(config)#enable secret Cisco
R1(config)#do sh run | i user
username admin secret 5 $1$mERr$vTbHul1N28cEp8lkLqr0f/ # Зашифрованный пароль
R1#
%SYS-5-CONFIG_I: Configured from console by console
R1#wr
Building configuration...
[OK]
R1#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
R1(config)#ip domain-name netology.ru
R1(config)#crypto key generate rsa
The name for the keys will be: R1.netology.ru
Choose the size of the key modulus in the range of 360 to 4096 for your
  General Purpose Keys. Choosing a key modulus greater than 512 may take
  a few minutes.

How many bits in the modulus [512]: 2048
% Generating 2048 bit RSA keys, keys will be non-exportable...[OK]
R1(config)#exit
R1#
%SYS-5-CONFIG_I: Configured from console by console
wr
Building configuration...
[OK]
```
# ![image7](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/7.PNG)

# ![image8](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/8.PNG)


# Авторизация Swich

`User` - admin
`pass` - admin
`enable` - Cisco

# Авторизация Router
`User` - admin
`pass` - admin
`enable` - Cisco
# ![image8](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/9.PNG)
