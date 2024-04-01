# `Настройка коммутации и маршрутизации`

# Исполнитель: Смирнов Кирилл NTW-28
### Цель задания
1. Настроить коммутацию.
2. Настроить маршрутизацию.

------



### Лабораторная работа "Настройка коммутации и маршрутизации"

Соберите в среде Packet Tracer приведенную на схеме топологию и выполните задания.


![image](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/99.PNG)
------

### Задание 1. 

Создать VLANы пользователей 100, 200, 300 на коммутаторах. Создать соответствующие VLAN интерфейсы. Назначить IP адреса на АРМ пользователей. Проверить связанность между пользователями внутри VLAN, между разными VLAN и доступность шлюзов в каждом VLAN. 

*Результат представить в виде вывода команд `show vlan brief` и `show interface trunk` с коммутаторов SW_1, SW_2, SW_3.*

------

# Ответ  [Файл.pkt](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/%D0%93%D0%BE%D1%82%D0%BE%D0%B2%D0%BE%D0%B5.pkt)

# В данном задании дополнил настройку ACL стандартных листов, что бы вланы не пинговались между собою через Коммутатор L2+

## Создание VLAN на Switch 0 и Switch1

```
Switch>enable 

Switch#configure terminal 
Switch(config)#vlan 100
Switch(config-vlan)#name USER_1
Switch(config-vlan)#exit
Switch(config)#interface fastEthernet 0/1
Switch(config-if)#switchport mode access 
Switch(config-if)#switchport access vlan 100
Switch(config-if)#exit
Switch(config)#vlan 200
Switch(config-vlan)#name USER_2
Switch(config-vlan)#exit
Switch(config)#interface fastEthernet 0/2
Switch(config-if)#switchport mode access 
Switch(config-if)#switchport access vlan 200
Switch(config-if)#exit
Switch(config)#vlan 300
Switch(config-vlan)#name USER_3
Switch(config-vlan)#exit
Switch(config)#interface fastEthernet 0/3
Switch(config-if)#switchport mode access 
Switch(config-if)#switchport access vlan 300
Switch(config-if)#exit
Switch(config)#vlan 999
Switch(config-vlan)#name MGMT
Switch(config-vlan)#exit
Switch(config)#interface fastEthernet 0/24
Switch(config-if)#switchport mode trunk 
Switch(config-if)#switchport trunk allowed 100,200,300,999
Switch(config-if)#exit
Switch(config)#exit
```
# Создание интерфейсаVlan999 на Switch0
```
Switch>enable 
Switch#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#interface vlan 999
Switch(config-if)#
%LINK-5-CHANGED: Interface Vlan999, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan999, changed state to up

Switch(config-if)#ip address 10.10.0.195 255.255.255.192
Switch(config-if)#
Switch#
````
# Создание интерфейсаVlan999 на Switch0 
```
Switch>enable 
Switch#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#interface vlan 999
Switch(config-if)#
%LINK-5-CHANGED: Interface Vlan999, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan999, changed state to up

Switch(config-if)#ip address 10.10.0.194 255.255.255.192
Switch(config-if)#
Switch#
````




## Создание VLAN на Multilayer Switch0
```
Switch#enable 
Switch#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#ip routing
Switch(config)#ip routing 
Switch(config)#vlan 100
Switch(config-vlan)#
%LINK-5-CHANGED: Interface Vlan100, changed state to up
Switch(config-vlan)#name USER_1
Switch(config-vlan)#exit
Switch(config)#
Switch(config)#vlan 200
Switch(config-vlan)#
%LINK-5-CHANGED: Interface Vlan200, changed state to up
Switch(config-vlan)#name USER_2
Switch(config-vlan)#exit
Switch(config)#
Switch(config)#vlan 300
Switch(config-vlan)#
%LINK-5-CHANGED: Interface Vlan300, changed state to up
Switch(config-vlan)#name USER_3
Switch(config-vlan)#exit
Switch(config)#
Switch(config)#vlan 999
Switch(config-vlan)#
%LINK-5-CHANGED: Interface Vlan999, changed state to up
Switch(config-vlan)#name MGMT
Switch(config-vlan)#exit
```

# Настройка vlan интерфейсов на Multilayer Switch0
```
Switch#configure terminal 
Switch(config)#interface vlan 100
Switch(config-if)#ip address 10.10.0.1 255.255.255.192
Switch(config-if)#no shutdown 
Switch(config-if)#exit
Switch(config)#interface vlan 200
Switch(config-if)#ip address 10.10.0.65 255.255.255.192
Switch(config-if)#no shutdown 
Switch(config-if)#exit
Switch(config)#interface vlan 200
Switch(config-if)#ip address 10.10.0.129 255.255.255.192
Switch(config-if)#no shutdown 
Switch(config-if)#exit
Switch(config)#interface vlan 999
Switch(config-if)#ip address 10.10.0.193 255.255.255.192
Switch(config-if)#no shutdown 
Switch(config-if)#exit
Switch#ip routing
Switch#ip routing
```


# Настройка ACL листа на Multilayer Switch0
```
Switch#conf
Switch#configure terminal 
Switch(config)#ip access-list standard 1
Switch(config-std-nacl)#deny host 10.10.0.65
Switch(config-std-nacl)#deny host 10.10.0.66
Switch(config-std-nacl)#deny host 10.10.0.67
Switch(config-std-nacl)#deny host 10.10.0.129
Switch(config-std-nacl)#deny host 10.10.0.130
Switch(config-std-nacl)#deny host 10.10.0.131
Switch(config-std-nacl)#permit any
Switch(config-std-nacl)#exit
Switch(config)#ip access-list standard 2
Switch(config-std-nacl)#deny host 10.10.0.1
Switch(config-std-nacl)#deny host 10.10.0.2
Switch(config-std-nacl)#deny host 10.10.0.3
Switch(config-std-nacl)#deny host 10.10.0.129
Switch(config-std-nacl)#deny host 10.10.0.130
Switch(config-std-nacl)#deny host 10.10.0.131
Switch(config-std-nacl)#permit any
Switch(config-std-nacl)#exit
Switch(config)#ip access-list standard 3
Switch(config-std-nacl)#deny host 10.10.0.1
Switch(config-std-nacl)#deny host 10.10.0.2
Switch(config-std-nacl)#deny host 10.10.0.3
Switch(config-std-nacl)#deny host 10.10.0.65
Switch(config-std-nacl)#deny host 10.10.0.66
Switch(config-std-nacl)#deny host 10.10.0.67
Switch(config-std-nacl)#permit any
Switch(config-std-nacl)#exit
Switch(config)#exit
Switch#
```


# Назначим ACL листы на Multilayer Switch0
```
witch#conf tr
Switch#conf ter
Switch#conf terminal 
Switch(config)#interface vlan 100
Switch(config-if)#ip access-group 1
Switch(config-if)#ip access-group 1 in
Switch(config-if)#ip access-group 1 out 
Switch(config-if)#exit
Switch(config)#interface vlan 200
Switch(config-if)#ip access-group 2
Switch(config-if)#ip access-group 2 in
Switch(config-if)#ip access-group 2 out 
Switch(config-if)#exit
Switch(config)#interface vlan 300
Switch(config-if)#ip access-group 3
Switch(config-if)#ip access-group 3 in
Switch(config-if)#ip access-group 3 out 
Switch(config-if)#exit
```
# ВНИМАНИЕ ! dany any должо наъодиться в конце списка 

## Если что удаляем и заново вносим правило
`Пример` 

````
Switch#configure terminal 
Switch(config)#ip access-list standard 1
Switch(config-std-nacl)#no deny host 10.10.0.65
Switch(config-std-nacl)#no deny host 10.10.0.66
Switch(config-std-nacl)#no deny host 10.10.0.67
Switch(config-std-nacl)#deny host 10.10.0.129
Switch(config-std-nacl)#deny host 10.10.0.130
Switch(config-std-nacl)#deny host 10.10.0.131
Switch(config-std-nacl)#no permit any
Switch(config-std-nacl)#permit any
````
# Скриншоты команд `show vlan brief` и `show interface trunk` с коммутаторов Switch0, Switch1, Multilayer Switch0.

`Switch0`

# ![images1](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/1.PNG)

# ![images2](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/2.PNG)

`Switch1`

# ![images3](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/3.PNG)

# ![images4](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/4.PNG)

`Multilayer Switch0`

# ![images5](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/5.PNG)

# ![images6](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/6.PNG)

### ACL лист 

# ![images7](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/7.PNG)

### Задание 2.

Настроить связанность между SW_3 и R_1. Настроить связанность между SRV_100 и R_1. 

*Результат представить в виде вывода команды с R_1 `show ip route` и с SW_3 `show vlan brief` и `show ip route`.*

---

# Ответ [Файл.pkt](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/%D0%93%D0%BE%D1%82%D0%BE%D0%B2%D0%BE%D0%B5.pkt)

# Настройка итерфейса на Router0
```
Router>enable 
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gigabitEthernet 0/1
Router(config-if)#ip address 192.168.0.1 255.255.255.252
Router(config-if)#no shutdown 
```
# Настройка итерфейса на  Multilayer Switch0

```Switch(config)#interface gigabitEthernet 0/1
Switch(config-if)#no switchport 
Switch(config-if)# ip address 192.168.0.2 255.255.255.252

Switch#

```

# ! Дополнительно! Можно создать связанность через стыковочную сеть, без вывода порта из switchport
`пример создания влана и назначение его интерфейсу`
```
Switch(config)#vlan *** 
Switch(config-vlan)#
%LINK-5-CHANGED: Interface Vlan***, changed state to up
Switch(config-vlan)#name ***
Switch(config-vlan)#exit
Switch(config)#interface vlan ***
Switch(config-if)#ip address 192.168.0.2 255.255.255.252
Switch(config-if)#exit
Switch(config)#interface gigabitEthernet 0/1
Switch(config-if)#switchport mode access 
Switch(config-if)#switchport access vlan ***
Switch(config-if)#exit
```

# Настройка маршутов

### Настройка статической маршрутизации на Router0

```
Router#enable 
Router#configure ter
Router(config)#ip route 10.10.0.0 255.255.255.192 192.168.0.2
Router(config)#ip route 10.10.0.64 255.255.255.192 192.168.0.2
Router(config)#ip route 10.10.0.128 255.255.255.192 192.168.0.2
Router(config)#exit
```
### Настройка статической маршрутизации на  Multilayer Switch0

```
Switch#enable 
Switch#configure ter
Switch(config)#ip route 0.0.0.0 0.0.0.0 192.168.0.1
Switch(config)#exit
```

# Настроить связанность между Server (10.100.0.2/24) и Router0.

### Настройка итерфейса на Router0
```
Router>enable 
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gigabitEthernet 0/0
Router(config-if)#ip address 10.100.0.1 255.255.255.0
Router(config-if)#no shutdown 
```
### и настройка соответственно сервера, роверить досткпность можно с любого хоста через браузер


# Скриншоты вывода команды с `Router0` `show ip route` и с `Multilayer Switch0` show `vlan brief` и `show ip route`.

# ![images8](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/8.PNG)

# ![images9](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/9.PNG)

# ![images10](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/10.PNG)

### Задание 3.

Настроить связанность на Site_3 аналогично Site_1 и проверить связанность между серверами SRV_1 и 2, а так же доступность R_2 с серверов.

*Результат представить в виде вывода команд *`show ip route` с R_2 и `show vlan brief` с SW_5.*

---
# Ответ [Файл.pkt](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/%D0%93%D0%BE%D1%82%D0%BE%D0%B2%D0%BE%D0%B5.pkt)  
### Настройка итерфейса на Router0 для связанности с Router1
```
Router>enable 
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gigabitEthernet 0/2
Router(config-if)#ip address 192.168.0.5 255.255.255.252
Router(config-if)#no shutdown 
```

### Настройка итерфейсов на Router1
```
Router>enable 
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gigabitEthernet 0/2
Router(config-if)#ip address 192.168.0.3 255.255.255.252
Router(config-if)#no shutdown 
Router(config)#interface gigabitEthernet 0/1
Router(config-if)#ip address 192.168.0.9 255.255.255.252
Router(config-if)#no shutdown 
```
### Настройка статической маршрутизации на Router1

```
Router#enable 
Router#configure ter
Router(config)#ip route 10.10.0.0 255.255.255.192 192.168.0.5
Router(config)#ip route 10.10.0.64 255.255.255.192 192.168.0.5
Router(config)#ip route 10.10.0.128 255.255.255.192 192.168.0.5
Router(config)#ip route 10.100.0.0 255.255.255.0 192.168.0.5
Router(config)#ip route 10.11.0.0 255.255.255.128 192.168.0.10
Router(config)#ip route 10.11.0.128 255.255.255.192 192.168.0.10
Router(config)#exit
```



## Создание VLAN на Multilayer Switch1

```
Switch>enable 
Switch#configure terminal 
Switch(config)#vlan 661
Switch(config-vlan)#name SRV_1
Switch(config-vlan)#exit
Switch(config)#interface fastEthernet 0/2
Switch(config-if)#switchport mode access 
Switch(config-if)#switchport access vlan 661
Switch(config-if)#exit
Switch(config)#vlan 662
Switch(config-vlan)#name SRV_2
Switch(config-vlan)#exit
Switch(config)#interface fastEthernet 0/1
Switch(config-if)#switchport mode access 
Switch(config-if)#switchport access vlan 662
Switch(config-if)#exit
```
# Настройка vlan интерфейсов на Multilayer Switch1
```
Switch#configure terminal 
Switch(config)#interface vlan 661
Switch(config-if)#ip address 10.11.0.1 255.255.255.128
Switch(config-if)#no shutdown 
Switch(config-if)#exit
Switch(config)#interface vlan 662
Switch(config-if)#ip address 10.11.0.129 255.255.255.192
Switch(config-if)#no shutdown 
Switch(config-if)#exit
Switch#ip routing
```

# Настройка итерфейса на  Multilayer Switch1

```Switch(config)#interface gigabitEthernet 0/1
Switch(config-if)#no switchport 
Switch(config-if)# ip address 192.168.0.10 255.255.255.252
Switch#

```

### Настройка статической маршрутизации на  Multilayer Switch1

```
Switch#enable 
Switch#configure ter
Switch(config)#ip route 0.0.0.0 0.0.0.0 192.168.0.9
Switch(config)#ip route 10.10.0.0 255.255.255.192 192.168.0.9
witch(config)#ip route 10.10.0.64 255.255.255.192 192.168.0.9
witch(config)#ip route 10.10.0.128 255.255.255.192 192.168.0.9
witch(config)#ip route 10.100.0.0 255.255.255.0 192.168.0.9
Switch(config)#exit
```
# Скриншоты вывода команд `show ip route` с `Router1` и `show vlan brief` с `Multilayer Switch1`.

# ![images11](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/11.PNG)

# ![images12](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/12.PNG)


## Дополнительное задание (со звездочкой*)

Эти задания дополнительные (не обязательные к выполнению) и никак не повлияют на получение вами зачета по этому домашнему заданию. Вы можете их выполнить, если хотите глубже и/или шире разобраться в материале.

### Задание 4*.

С помощью статических маршрутов на R_1 и R_2 обеспечьте связанность между всеми сегментами на всех Site.

*Результат представить в виде вывода команд `show ip route` с R_1 и R_2*

# Ответ [Файл.pkt](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/%D0%93%D0%BE%D1%82%D0%BE%D0%B2%D0%BE%D0%B5.pkt)
### Настройка маршутов

### Настройка статической маршрутизации на Router0

```
Router#enable 
Router#configure ter
Router(config)#ip route 10.11.0.0 255.255.255.128 192.168.0.6
Router(config)#ip route 10.11.0.128 255.255.255.192 192.168.0.6
Router(config)#exit

```

### Маршуты на Router1 настраиватлись в предыдущем задании 

# Скриншоты вывода команд `show ip route` с Router0 и Router1


# ![images13](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/13.PNG)



# ![images14](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/14.PNG)
