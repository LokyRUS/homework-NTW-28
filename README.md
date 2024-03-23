# `Путь пакета в коммутируемой и маршрутизируемой среде. ARP, DHCP, DNS.`
# Исполнитель: Смирнов Кирилл NTW-28

### Цель задания

Понять принцип передвижения пакета и взаимодействия основных протоколов.
---

### Задание 1.

На основном интерфейсе вашего ПК (или виртуальной машины) посмотрите дамп трафика через tshark или Wireshark.

*Приведите скриншоты, где показаны основные моменты работы протокола ARP и все этапы работы DHCP-протокола.*

---
# Ответ
`Работа протокола arp`
# ![images1](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/1.PNG)

`Работа протокола DNS`
# ![images2](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/2.PNG)

# ![images3](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/3.PNG)

`Принудительное обращение к DNS Google 8.8.8.8`
# ![images2](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/4.PNG)

### Задание 2.

Как настраивается для сетевого пакета размер MTU в сетях, использующих только протокол IPv6?

*Приведите ответ в свободной форме.*
# Ответ

В сетях ipv6 MTU настраиватеся в процессе согласования скоростей. Минимальный размер MTU у ipv6 составляет 1280 байт, а во многих сетях используется mtu 1500. Однако, если встретиться сеть с меньшим MTU ,то будет использована технология Path MTU Discovery.Это когда IPv6 пакет можно разбить на меньшие пакеты только на узле отправителя и сборка пакета может выполняться только на узле получателя и никакой фрагментации. 
Например, в ipv4 максимальный MTU равен 576 байт, и в случае фрагментации, если пакет слишком велик для его передачи по каналу связи, узел отправителя может разбить его на несколько фрагментов.

---

### Задание 3. Лабораторная работа "Построение сети и разбор передаваемых в ней пакетов"

1. В Cisco Packet Tracer соберите сеть состоящую из двух маршрутизаторов (R1 и R2), за каждым из которых есть коммутатор (Switch1 и Switch2), а за коммутатором по два компьютера (Comp1, Comp2 и Comp3, Comp4). Все устройства этой сети должны быть доступны между собой.

Сетевые настройки можно использовать следующие:

  - R1 10.1.1.1/27
  - R2 10.1.2.1/27

  *Приведите скриншоты таблицы коммутации и таблицы маршрутизации устройств R1, R2, Switch1, Switch2.
   Пришлите pkt файл.*

2. Как будут выглядеть заголовки пакета на каждом из узловых точек сети из первой части лабораторного задания при обмене данными Comp1 с Comp4? 

  *Приведите ответ в свободной форме.*

---
# Ответ

# Настроим dhcp сервер на роутерах для раздачи ip адресов для наших pc
`Router0`
```
Router>en
Router>enable 
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gigabitEthernet 0/0/0
Router(config-if)#ip address 10.1.1.1 255.255.255.224
Router(config-if)#no shutdown 
Router(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/0, changed state to up
Router(config-if)#exit
Router(config)#ip dhcp pool Zona1
Router(dhcp-config)#network 10.1.1.0 255.255.255.224
Router(dhcp-config)#default-router 10.1.1.1
Router(dhcp-config)#dns-server 8.8.8.8
Router(dhcp-config)#exit
Router(config)#ip dhcp  excluded-address 10.1.1.1
Router(config)#exit
Router#
%SYS-5-CONFIG_I: Configured from console by console

Router#
Router#
```

`Router0`
```
Router>en
Router>enable 
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gigabitEthernet 0/0/0
Router(config-if)#ip address 10.1.2.1 255.255.255.224
Router(config-if)#no shutdown 
Router(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/0, changed state to up
Router(config-if)#exit
Router(config)#ip dhcp pool Zona2
Router(dhcp-config)#network 10.1.2.0 255.255.255.224
Router(dhcp-config)#default-router 10.1.2.1
Router(dhcp-config)#dns-server 8.8.8.8
Router(dhcp-config)#exit
Router(config)#ip dhcp  excluded-address 10.1.2.1
Router(config)#exit
Router#
%SYS-5-CONFIG_I: Configured from console by console

Router#
Router#
```

# Наcтроим маршрутные интерфейсы и настроим маршрутизацю на роутерах.
`Router0`
```
Router#en
Router#enable 
Router#configure ter
Router(config)#interface gigabitEthernet 0/0/1
Router(config-if)# ip address 10.1.10.1 255.255.255.252
Router(config-if)#no shutdown 
Router(config-if)#exit
Router(config)#ip route 10.1.2.0 255.255.255.224 10.1.10.2
Router(config)#exit

```

`Router1`
```
Router#en
Router#enable 
Router#configure ter
Router(config)#interface gigabitEthernet 0/0/1
Router(config-if)# ip address 10.1.10.2 255.255.255.252
Router(config-if)#no shutdown 
Router(config-if)#exit
Router(config)#ip route 10.1.1.0 255.255.255.224 10.1.10.1
Router(config)#exit

```

# Таблица коммутации и таблица маршрутизации

# ![images5](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/5.PNG)

1. этап. При обмене данными на первом этапе L2 уровня, в рамках локальной мети, будет происходить обмен пакетами на уровне mac адресации как видно из примера, отправленный пакет с хоста адресован мак адресу Роутера, потому хост уже знает, запрашиваемый  им Ip адрес находится ЗА указанным им мак адресом. И пакет идёт в направлении коммутатора
# ![images6](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/6.PNG)

2. Далее идёт отправка пакета в направлении роутера от коммутатора с его макадресом назначения и происходит дальнейшая инкапсуляция. 

# ![images7](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/7.PNG)

3. На L3 уровне отправка осуществляется на адрес по маршруту с использованием Ip адресации, за которым находится адрес назначения.

# ![images8](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/8.PNG)

4. Далее происходит декапсуляция пакета и цикл повторяется в обратном направлении, но с учётом ответа адреса назначения.


## Дополнительные задания (со звездочкой*)

Эти задания дополнительные (не обязательные к выполнению) и никак не повлияют на получение вами зачета по этому домашнему заданию. Вы можете их выполнить, если хотите глубже и/или шире разобраться в материале.

---

### Задание 4*.

1. Загрузите предыдущую лабораторную работу  из  задания №3.
2. Добавьте сервер к любому коммутатору.
3. Назначьте ему IP-адрес из той же подсети и включите сервис HTTP.
4. Поднимите на этом сервере сервис DNS и сделайте А-запись: "A запись - netology.ru".
5. На маршрутизаторе, в сети которого присутствует сервер DNS, настройте выдачу IP-адресов по протоколу DHCP. Также необходимо, чтобы по DHCP клиентам выдавался IP-адрес DNS-сервера.
6. С любого компьютера зайдите на сайт netology.ru

*Приведите скриншоты, где с помощью утилиты nslookup можно получить IP адрес сайта netology.ru.*

