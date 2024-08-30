# Домашнее задание к занятию "Элементы БЛВС. Точки доступа, контроллеры, антенны"
# Исполнитель: Смирнов К.Е.
### Цель задания

В результате выполнения этого задания вы:

1) Научитесь конфигурировать WLC c различными группа AP.
2) Научитесь создавать различные WLAN и привязывать их к WLC.
3) Научитесь подключать клиентов к различным WLAN.

---

### Лабораторная работа "Конфигурация AP Group, WLANs на Cisco WLC"

![](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/99.PNG)
Соберите аналогичную топологию в Cisco Packet Tracer.   

Критерии выполнения:
1. На WLC необходимо создать дополнительные AP Group. AP1 должен быть в группе WIFI_Group1, AP2 в группе WIFI_Group2.
2. Должны быть созданы два Wireless Lans: WIFI_Default и WIFI2. Оба WLAN должны имет протокол аутентификации - WPA_PSK.
3. На AP1 должен раздавать только WIFI_Default, на AP2 - только WIFI2.
4. Один smartphone должен подключаться к WIFI_Default, другой к WIFI2.

Замечение: не забывайте про конфигурацию DHCP сервера на маршрутизаторе для раздачи адресов.

*Отправьте готовый файл pkt*

---
# Ответ

# [Скачать файл.pkt](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/zadanie.pkt)

# Разделение на VLAN

|VLAN| name |network|прочее|
| --- | --- | --- |---|
| vlan10 | MGMT | 192.168.10.0/24 |контроллер , маршрутизатор, точки доступа|
| vlan20 | WIFI_Default | 192.168.20.0/24  |AP1|
| vlan30 | WIFI2 | 192.168.30.0/24 |AP2|

# Настройка транка с вланами
```
Switch>enable 
Switch#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#vlan 10
Switch(config-vlan)#name MGMT 
Switch(config-vlan)#ex
Switch(config)#vlan 20
Switch(config-vlan)#name WIFI_Default
Switch(config-vlan)#ex
Switch(config)#vlan 30
Switch(config-vlan)#name WIFI2
Switch(config-vlan)#ex
Switch(config)#interface gigabitEthernet 0/1
Switch(config-if)#switchport mode trunk 
Switch(config-if)#switchport trunk allowed vlan 10,20,30
Switch(config-if)#

```
# ![images 1](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/1.PNG)

# Создание сабинтерфейсов на роутере

```
Router>enable 
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gigabitEthernet 0/0/0
Router(config-if)#no shutdown 
Router(config-if)#interface gigabitEthernet 0/0/0.10
Router(config-subif)#encapsulation dot1Q 10
Router(config-subif)#ip address 192.168.10.254 255.255.255.0
Router(config-subif)#ex
Router(config)#interface gigabitEthernet 0/0/0.20
Router(config-subif)#encapsulation dot1Q 20
Router(config-subif)#ip address 192.168.20.254 255.255.255.0
Router(config-subif)#ex
Router(config)#interface gigabitEthernet 0/0/0.30
Router(config-subif)#encapsulation dot1Q 30
Router(config-subif)#ip address 192.168.30.254 255.255.255.0
Router(config-subif)#ex
```
# ![images 2](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/2.PNG)

# Назначаем порт в сторону контроллера к аксес режиме на Vlan 10

```
Switch>en
Switch>enable 
Switch#configure terminal 
Switch(config)#interface fastEthernet 0/1
Switch(config-if)#switchport mode access 
Switch(config-if)#switchport access vlan 10
Switch(config-if)#ex
```
# Назначаем Ip адрес контроллеру 

# ![images 3](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/3.PNG)

# Настраивам транк для точек доступа  для vlan 10,20,30 и назначаем netive (корневым или родным) vlan 10 (влан облуживания)
```
Switch#configure terminal 
Switch(config)#interface range fastEthernet 0/2-3
Switch(config-if-range)#switchport mode trunk 
Switch(config-if-range)#switchport trunk allowed vlan 10,20,30
Switch(config-if-range)#switchport trunk native vlan 10

```

# Настройка DHCP на роутере с настрокой опции 43 для взаидомейсвия контроллера и точек доступа. 

```
Router>enable 
Router#configure ter
Router(config)#ip dhcp pool MGMT
Router(dhcp-config)#network 192.168.10.0 255.255.255.0
Router(dhcp-config)#default-router 192.168.10.254
Router(dhcp-config)#option 43 ip 192.168.10.50
Router(dhcp-config)#
```
# Настройка DHCP на роутере для wifi сетей.

### Для WIFI_Default

```
 Router>enable 
Router#configure ter
Router(config)#ip dhcp pool WIFI_Default
Router(dhcp-config)#network 192.168.20.0 255.255.255.0
Router(dhcp-config)#default-router 192.168.20.254
Router(dhcp-config)#
```
### Для WIFI2

```
 Router>enable 
Router#configure ter
Router(config)#ip dhcp pool WIFI2
Router(dhcp-config)#network 192.168.30.0 255.255.255.0
Router(dhcp-config)#default-router 192.168.30.254
Router(dhcp-config)#
```


# ![images 4](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/4.PNG)

# ![images 5](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/5.PNG)

# Создаем беспроводные сети 

### WIFI_Default

# ![images 6](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/6.PNG)

### WIFI2

# ![images 7](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/7.PNG)

# Создание групп 

### Группа  `WIFI_Group1`

# ![images 8](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/8.PNG)

### Группа  `WIFI_Group2`

# ![images 9](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/9.PNG)

# подключаем смартфоны 

### К сети WIFI_Default

# ![images 10](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/10.PNG)

### К сети WIFI2

# ![images 11](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/11.PNG)

# Получившаяся топология 

# ![images 12](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/12.PNG)

# [Скачать файл.pkt](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/zadanie.pkt)

### Правила приема домашнего задания

В личном кабинете отправлен pkt-файл с выполненным проектом.


### Критерии оценки

Зачет - выполнены все задания, ответы даны в развернутой форме, приложены соответствующие скриншоты и файлы проекта, в выполненных заданиях нет противоречий и нарушения логики.

На доработку - задание выполнено частично или не выполнено, в логике выполнения заданий есть противоречия, существенные недостатки.
