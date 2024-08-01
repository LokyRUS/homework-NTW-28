# Домашнее задание к занятию "Основные средства защиты от сетевых атак на коммутаторах."

### Цель задания

Данное домашнее задание позволит научиться использовать технологии защиты на L2 коммутаторов от популярных атак. Данные знания очень важны для постороение сетей и прохождения сертификации безопасности PCI-DSS, которую проходят банки.

В результате выполнения задания вы:
1) Научитесь конфигурировать DHCP Snooping. Понимать принципы работы. 
2) Научитесь защищаться от атак по смене IP адреса при помощи IP Source Guard.
3) Научитесь защищать протокол ARP от подмены сетевой информации со стороны злоумышленника.

------

### Лабораторная работа "Конфигурация DHCP Snooping и ARP Inspection"

Можете использовать симулятор для построения топологии и проверки/поиска ответов на задания. 

------

### Задание 1. 

На картинке изображена схема офисной сети:
![image](https://user-images.githubusercontent.com/51816695/160147812-5bd15814-762e-4cec-b27e-e8a601f461da.png)

Каким образом необходимо настроить все 4 свитча, чтобы в сети корректно работал только один DHCP сервер - маршрутизатор R1.
Очень важна корректная конфигурация портов на всех свитчах.

*Перечислите список свитчей и список команд, которые необходимо выполнить.*

------
# Ответ
## Построенная топология в CPT
# ![image 1]()
# [скачать файл.pkt]()

## ! Настройки описываются в соответсвии с топологией из условий задания, но на основе работающей топологии из CPT. ( Наименование интерфейсов будут со скрина из условий)

### 1. Настройка DHCP сервера `R1`
```concole
Router#configure terminal
Router(config)#interface GigabitEthernet0/0
Router(config-if)#ip address 192.168.0.1 255.255.255.0
Router(config-if)#exit
Router(config)#interface GigabitEthernet0/0
Router(config-if)#no shutdown
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up
%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up
Router(config)#ip dhcp pool IPD
Router(dhcp-config)#network 192.168.0.0 255.255.255.0
Router(dhcp-config)#default-router 192.168.0.1
Router(config)#ip dhcp excluded-address 192.168.0.1 192.168.0.10
Router(config)#
```
### 2. Настройка ложного DHCP сервера `R2`
```console
Router#configure terminal
Router(config)#interface GigabitEthernet0/0
Router(config-if)#ip address 192.168.100.1 255.255.255.0
Router(config-if)#exit
Router(config)#interface GigabitEthernet0/0
Router(config-if)#no shutdown
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up
%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up
Router(config)#ip dhcp pool IPD
Router(dhcp-config)#network 192.168.100.0 255.255.255.0
Router(dhcp-config)#default-router 192.168.100.1
Router(config)#ip dhcp excluded-address 192.168.100.1 192.168.100.10
Router(config)#
```
## Вклучение `ip dhcp snooping` и перевод портов в `Trust` 

```
Switch>enable 
Switch#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#ip dhcp snooping 
Switch(config)#interface fastEthernet 0/3
Switch(config-if)#ip dhcp snooping trust 
Switch(config-if)#exit
Switch(config)#ex
Switch#
```
### Задание 2. 

По топологии из задания 1 необходимо на SW2 настроить ARP Inspection и IP Source guard для Client9 и Client10, подключенных к SW2.
Client9 получает адрес по DHCP, Client10 ip адрес задан статически. DHCP Snooping уже настроен по первому заданию.

*Перечислите список команд, которые необходимо применить на SW2*


### Правила приема домашнего задания

В личном кабинете отправлена ссылка на документ (Google Doc) с выполненным заданием. В документе настроены права доступа “Просматривать могут все в Интернете, у кого есть ссылка”.

### Критерии оценки

Зачет - выполнены все задания, ответы даны в развернутой форме, приложены соответствующие скриншоты и файлы проекта, в выполненных заданиях нет противоречий и нарушения логики.

На доработку - задание выполнено частично или не выполнено, в логике выполнения заданий есть противоречия, существенные недостатки.
