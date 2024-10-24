# Домашнее задание к занятию "Проектирование WAN сегмента. Топологии и принципы построения"

### Цель задания

В результате выполнения этого задания вы:

1. Научитесь использовать технологию GRE для построения корпоративной сети
2. Научитесь настраивать динамическую маршрутизацию для работы GRE-туннелей

---

### Инструкция к выполнению домашнего задания
1. Скачайте [Шаблон для домашнего задания](https://u.netology.ru/backend/uploads/lms/content_assets/file/281/%D0%A1%D0%94%D0%95%D0%9B%D0%90%D0%99%D0%A2%D0%95_%D0%9A%D0%9E%D0%9F%D0%98%D0%AE_-_%D0%A8%D0%B0%D0%B1%D0%BB%D0%BE%D0%BD_%D0%B4%D0%BB%D1%8F_%D0%B4%D0%BE%D0%BC%D0%B0%D1%88%D0%BD%D0%B5%D0%B3%D0%BE_%D0%B7%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D1%8F_1.1._%D0%9D%D0%B0%D0%B7%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5_%D0%BB%D0%B5%D0%BA%D1%86%D0%B8%D0%B8_-_%D0%A4%D0%B0%D0%BC%D0%B8%D0%BB%D0%B8%D1%8F_%D0%98%D0%BC%D1%8F.docx) на своё устройство.
2. Откройте скачанный файл на личном диске в Google.
3. В названии файла введите корректное название лекции и ваши фамилию и имя.
4. Зайдите в «Настройки доступа» и выберите доступ «Просматривать могут все в интернете, у кого есть ссылка». Инструкция «Как предоставить доступ к файлам и папкам на Google Диске» [по ссылке](https://support.google.com/docs/answer/2494822?hl=ru&co=GENIE.Platform%3DDesktop).
5. Скопируйте текст задания в свой документ.
6. Выполните задание, запишите ответы и приложите необходимые скриншоты в свой Google-документ.
7. Для проверки домашнего задания отправьте ссылку на ваш Google-документ в личном кабинете.
8. Любые вопросы по решению задач можно задать в чате учебной группы, в чате поддержки или в разделе «Вопросы по заданию» в личном кабинете.
9. Подробнее о работе с Google-документами и загрузке решения на проверку можно найти в [«Руководстве по работе с материалами для обучения»](https://l.netology.ru/instruktsiya-po-materialami-dlya-obucheniya)

---

### Лабораторная работа "Построение корпоративной сети с использованием технологии GRE"

Топология сети для выполнения лабораторной работы представлена на картинке ниже:
![homework](https://user-images.githubusercontent.com/40097402/179345904-f0576b79-c850-48d0-9b25-b2b9c4d5d9f4.jpg)

Описание: есть центральный офис с маршрутизаторами, на каждом из которых настроен выход в интернет. Есть удаленный офис со своим выходом в интеренет. Необходимо объединить корпоративную сеть посредством GRE туннеля и динамической маршрутизации.

### Задание 1. 

Разработать план IP-адресов для схемы сети выше. 

*Приведите ответ в свободной форме*

### Задание 2. 

Настроить протокол FHRP для резервирования default gateway. 

*Пришлите pkt-файл*

### Задание 3.

Поднять туннели GRE, используя все каналы выхода в интернет. 

*Пришлите pkt-файл*

### Задание 4.

Настроить динамическую маршрутизацию для работы GRE туннелей. 

*Пришлите pkt-файл и приложите краткое описание используемых решений*

# Ответ 

|Зона|Сеть|
|---|---|
|AS100|192.168.10.0/24|
|AS200|192.168.20.0/24|
|AS500|---|
|Router1--Routerinternet|10.0.1.0/24|
|Tunel0|10.10.20.0/24|  
|Router2--Routerinternet|10.0.2.0/24|
|Tunel0|10.10.10.0/24|  
|Router3--Routerinternet|10.0.3.0/24|
|Tunel0|10.10.10.0/24|  
|Tunel1|10.10.20.0/24|  

# Топология
![images1](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/1.PNG)

#[Скачать файл.pkt](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/zadanie.pkt)

# Настройка интерфейсов на router1 
```
Router#configure terminal in
Router(config)#interface gigabitEthernet 0/0/0
Router(config-if)#ip address 192.168.10.1 255.255.255.0
Router(config-if)#no shut
Router(config-if)#no shutdown 

Router(config)#interface gigabitEthernet 0/0/1
Router(config-if)#ip ad
Router(config-if)#ip address 10.0.1.2 255.255.255.0
Router(config-if)#no shu
Router(config-if)#no shutdown 
```

# Настройка интерфейсов на router2
```
Router2(config)#interface gigabitEthernet 0/0/0
Router2(config-if)#ip address 192.168.10.2 255.255.255.0
Router2(config-if)#no shutdown 
Router2(config)#interface gigabitEthernet 0/0/1
Router2(config-if)#ip address 10.0.2.2 255.255.255.0
Router2(config-if)#no shutdown 
```
# Настройка интерфейсов на router internet
```
Router#configure terminal in
Router(config)#interface gigabitEthernet 0/2
Router(config-if)#ip address 10.0.1.1 255.255.255.0
Router(config-if)#no shutdown 

Router(config)#interface gigabitEthernet 0/1
Router(config-if)#ip address 10.0.2.1 255.255.255.0
Router(config-if)#no shutdown 

Router(config)#interface gigabitEthernet 0/0
Router(config-if)#ip address 10.0.3.1 255.255.255.0
Router(config-if)#no shutdown 
```

# Настройка интерфейсов на router 4 
```
Router#configure terminal in
Router(config)#interface gigabitEthernet 0/0/0
Router(config-if)#ip address 10.0.3.2 255.255.255.0
Router(config-if)#no shutdown 

Router(config)#interface gigabitEthernet 0/0/1
Router(config-if)#ip address 192.168.20.1 255.255.255.0
Router(config-if)#no shutdown 
```
# Настройка HSRP на Router 1 
```
Router1(config)#interface gigabitEthernet 0/0/0
Router1(config-if)#standby 1 ip 192.168.10.1 
Router1(config-if)#standby 1 priority 100
Router2(config-if)#standby 1 preempt 
Router2(config-if)#standby 1 track gigabitEthernet 0/0/1
```
# Настройка HSRP на Router 2
```
Router2(config)#interface gigabitEthernet 0/0/0
Router2(config-if)#standby 1 ip 192.168.10.1
Router2(config-if)#standby 1 priority 105
Router2(config-if)#standby 1 preempt 
Router2(config-if)#standby 1 track gigabitEthernet 0/0/1
Router2(config-if)#ex
Router2(config)#

```
# Настройка BGP 

## Настройка на Router internet
```
Router>enable
Router#
Router#configure terminal
Router(config)#router bgp 500
Router(config-router)#network 10.0.1.0 mask 255.255.255.0
Router(config-router)#neighbor 10.0.1.2 remote-as 100
Router(config-router)#network 10.0.2.0 mask 255.255.255.0
Router(config-router)#neighbor 10.0.2.2 remote-as 100
Router(config-router)#network 10.0.3.0 mask 255.255.255.0
Router(config-router)#neighbor 10.0.3.2 remote-as 200
Router(config-router)#
```
## Настройка на Router1
```
Router>enable
Router#
Router#configure terminal
Router(config)#router bgp 100
Router(config-router)#network 10.0.1.0 mask 255.255.255.0
Router(config-router)#neighbor 10.0.1.1 remote-as 500
```
## Настройка на Router2

```
Router>enable
Router#
Router#configure terminal
Router(config)#router bgp 100
Router(config-router)#network 10.0.2.0 mask 255.255.255.0
Router(config-router)#neighbor 10.0.2.1 remote-as 500
```
## Настройка на Router3
```
Router(config-router)#network 10.0.3.0 mask 255.255.255.0
Router(config-router)#neighbor 10.0.3.1 remote-as 500
Router(config-router)#
```
# Создание тоннеля  и OSPF маршрутизации 

# Создание тоннеля на Router1

`Тоннель`
```
Router2#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router2(config)#interface tunnel 0

Router2(config-if)#
%LINK-5-CHANGED: Interface Tunnel0, changed state to up

Router2(config-if)#ip address 10.10.20.1 255.255.255.0
Router2(config-if)#tunnel source gigabitEthernet 0/0/1
Router2(config-if)#tunnel destination 10.0.3.2
Router2(config-if)#
%LINEPROTO-5-UPDOWN: Line protocol on Interface Tunnel0, changed state to up

```
`OSPF`

```
Router2(config)#router ospf 1
Router1(config-router)#network 192.168.10.0 0.0.0.255 area 0
Router1(config-router)#network 10.10.20.0 0.0.0.255 area 0
Router1(config-router)#
```
# Создание тоннеля на Router2

`Тоннель`
```
Router2#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router2(config)#interface tunnel 0

Router2(config-if)#
%LINK-5-CHANGED: Interface Tunnel0, changed state to up

Router2(config-if)#ip address 10.10.10.1 255.255.255.0
Router2(config-if)#tunnel source gigabitEthernet 0/0/1
Router2(config-if)#tunnel destination 10.0.3.2
Router2(config-if)#
%LINEPROTO-5-UPDOWN: Line protocol on Interface Tunnel0, changed state to up

```
`OSPF`

```
Router2(config)#router ospf 1
Router2(config-router)#network 192.168.10.0 0.0.0.255 area 0
Router2(config-router)#network 10.10.10.0 0.0.0.255 area 0   # тоннельная  сеть
Router2(config-router)#
```
# Создание тоннеля на Router3

`Тоннель 0 `
```
Router2#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router2(config)#interface tunnel 0

Router2(config-if)#
%LINK-5-CHANGED: Interface Tunnel0, changed state to up

Router2(config-if)#ip address 10.10.10.2 255.255.255.0
Router2(config-if)#tunnel source gigabitEthernet 0/0/0
Router2(config-if)#tunnel destination 10.0.2.2
Router2(config-if)#
%LINEPROTO-5-UPDOWN: Line protocol on Interface Tunnel0, changed state to up

```

`Тоннель 1 `

```
Router2#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router2(config)#interface tunnel 1

Router2(config-if)#
%LINK-5-CHANGED: Interface Tunnel1, changed state to up

Router2(config-if)#ip address 10.10.20.2 255.255.255.0
Router2(config-if)#tunnel source gigabitEthernet 0/0/0
Router2(config-if)#tunnel destination 10.0.1.2
Router2(config-if)#
%LINEPROTO-5-UPDOWN: Line protocol on Interface Tunnel0, changed state to up

```

`OSPF`

```
Router2(config)#router ospf 1
Router2(config-router)#network 192.168.10.0 0.0.0.255 area 0
Router2(config-router)#network 10.10.10.0 0.0.0.255 area 0
Router2(config-router)#network 10.10.20.0 0.0.0.255 area 0

```



### Правила приема домашнего задания

В личном кабинете отправлены:

- ссылка на документ (Google Doc) с выполненным заданием. В документе настроены права доступа “Просматривать могут все в Интернете, у кого есть ссылка”;
- файл в формате .pkt.

### Критерии оценки

Зачет - выполнены все задания, ответы даны в развернутой форме, приложены соответствующие скриншоты и файлы проекта, в выполненных заданиях нет противоречий и нарушения логики.

На доработку - задание выполнено частично или не выполнено, в логике выполнения заданий есть противоречия, существенные недостатки.
