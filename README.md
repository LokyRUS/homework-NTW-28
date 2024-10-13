# Домашнее задание к занятию "Подходы и методологии проектирования корпоративных сетей"

# Исполнитель: Смирнов К.Е.
### Цель задания

Научиться проектировать и создавать корпоративные сети с возможностью масштабирования

------

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

### Лабораторная работа "Построение корпоративной сети"

### Задание 1

Руководство организации, в которой вы работаете сообщило вам, что компания расширяется. В следующем месяце в планах открыть новые офисы в соседнем регионе.

Перед вами поставили следующие требования:

1. Будут 4 офиса: 1 центральный и 3 отдаленных мини-офиса.
2. В центральном офисе планируется разместить 35 сотрудников из следующих отделов:
- ТОП-менеджеры
- менеджеры по продажам
- IT-отдел
- служба безопасности
- бухгалтерия
3. В центральном офисе помимо рабочих станций будут: принтеры, устройства для подключения к сети Wi-Fi, камеры видеонаблюдения.
4. В остальных мини-офисах планируется разместить:
- в первом: производственный цех с 15 сотрудниками, их рабочие станции и принтер
- во втором: склад с 6 сотрудниками, их рабочие станции и камеры видеонаблюдения
- в третьем: магазин по продаже изготовленной продукции с 5 сотрудниками, их рабочие станции, принтер и кассовое устройство
5. При проектировании сети в Packet Tracer не требуется подключение всех оконечных устройств (компьютеров пользователей, периферийных устройств и так далее). 
Подключение по одному устройству каждого типа достаточно для проверки выполнения наличия связанности.
6. Сотрудники всех мини-офисов должны иметь доступ к инфраструктуре центрального офиса.
7. IPv6 адресация в работе не рассматривается.

В Cisco Packet Tracer необходимо спроектировать и настроить сеть согласно условиям задания. Составить адресный план. Число и функционал коммутаторов доступа вы определяете самостоятельно.

*Отправьте pkt-файл и приложите краткое описание используемых решений.*

# План

# Планирование цетрального офиса 
# `Сегиентирование локаьной сети` 
|VLAN|Пользователи|
|---|---|
|VLAN10|IT(IT-отдел) |
|VLAN20|manager (менеджеры по продажам)|
|VLAN30|topmanager (ТОП-менеджеры)|
|VLAN40|security (служба безопасности)|
|VLAN50|Buh (бухгалтерия)|

# `Сегиентирование беспроводной сети`
|VLAN|Пользователи|
|---|---|
|VLAN20|AP1|
|VLAN30|AP2|

|Устройство||||||
|---|---|---|---|---|---|

# Настройка свича IT Vlan 10

```
Switch#configure terminal 
Switch(config)#vlan 10
Switch(config)#interface fastEthernet 0/1
Switch(config-if)#switchport access vlan 10
Switch(config)#interface fastEthernet 0/3
Switch(config-if)#switchport access vlan 10
```
# Настройка свича менеджеров 
```
Switch#configure terminal 
Switch(config)#vlan 10
Switch(config)#vlan 20
Switch(config)#interface fastEthernet 0/1
Switch(config-if)#switchport mode trunk 
Switch(config-if)#switchport trunk native vlan 10
Switch(config-if)#switchport trunk allowed vlan 10,20
Switch(config-if)#ex
Switch(config)#interface fastEthernet 0/2
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 20 
```
# Настройка свича ТОП-Менеджеров 
```
Switch#configure terminal 
Switch(config)#vlan 10
Switch(config)#vlan 30
Switch(config)#interface fastEthernet 0/1
Switch(config-if)#switchport mode trunk 
Switch(config-if)#switchport trunk native vlan 10
Switch(config-if)#switchport trunk allowed vlan 10,30
Switch(config-if)#ex
Switch(config)#interface fastEthernet 0/2
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 30 
```
# Настройка свича Охраны 
```
Switch#configure terminal 
Switch(config)#vlan 10
Switch(config)#vlan 40
Switch(config)#interface fastEthernet 0/1
Switch(config-if)#switchport mode trunk 
Switch(config-if)#switchport trunk native vlan 10
Switch(config-if)#switchport trunk allowed vlan 10,40
Switch(config-if)#ex
Switch(config)#interface fastEthernet 0/2
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 40 
```

# Настройка свича Бухгалтерии 
```
Switch#configure terminal 
Switch(config)#vlan 10
Switch(config)#vlan 50
Switch(config)#interface fastEthernet 0/1
Switch(config-if)#switchport mode trunk 
Switch(config-if)#switchport trunk native vlan 10
Switch(config-if)#switchport trunk allowed vlan 10,50
Switch(config-if)#ex
Switch(config)#interface fastEthernet 0/2
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 50 
```
# Настройка свича Отделов общий

```
Switch(config)#vlan 10
Switch(config-vlan)#ex
Switch(config)#vlan 20
Switch(config-vlan)#ex
Switch(config)#vla
Switch(config)#vlan 30
Switch(config-vlan)#ex
Switch(config)#vlan 40
Switch(config-vlan)#ex
Switch(config)#vlan 50
Switch(config-vlan)#ex
Switch(config)#interface fastEthernet 0/1
Switch(config-if)#switchport trunk allowed vlan 10,20,30,40,50
Switch(config-if)#ex
Switch(config)#interface fastEthernet 0/2
Switch(config-if)#switchport access vlan 10
Switch(config)#ex
Switch(config)#interface fastEthernet 0/3
Switch(config-if)#switchport mode trunk 
Switch(config-if)#switchport trunk native vlan 10
Switch(config-if)#switchport trunk allowed vlan 10,20
Switch(config)#interface fastEthernet 0/4
Switch(config-if)#switchport mode trunk 
Switch(config-if)#switchport trunk native vlan 10
Switch(config-if)#switchport trunk allowed vlan 10,30
Switch(config)#interface fastEthernet 0/5
Switch(config-if)#switchport mode trunk 
Switch(config-if)#switchport trunk native vlan 10
Switch(config-if)#switchport trunk allowed vlan 10,40
Switch(config)#interface fastEthernet 0/6
Switch(config-if)#switchport mode trunk 
Switch(config-if)#switchport trunk native vlan 10
Switch(config-if)#switchport trunk allowed vlan 10,50
```
### Настройка DHCP На роутере 

```
Router#configure terminal 
Router(config)#ip dhcp pool IT_otdel
Router(dhcp-config)#network 192.168.10.0 255.255.255.0
Router(dhcp-config)#default-router 192.168.10.1
Router(dhcp-config)#option 43 ip 192.168.10.50
Router(config)#ip dhcp pool Manager
Router(dhcp-config)#network 192.168.20.0 255.255.255.0
Router(dhcp-config)#default-router 192.168.20.1
Router(dhcp-config)#ex
Router(config)#ip dhcp pool TOP_Manager
Router(dhcp-config)#network 192.168.30.0 255.255.255.0
Router(dhcp-config)#default-router 192.168.30.1
Router(dhcp-config)#
Router(config)#ip dhcp pool Security
Router(dhcp-config)#network 192.168.40.0 255.255.255.0
Router(dhcp-config)#default-router 192.168.40.1
Router(dhcp-config)#ex
Router(config)#ip dhcp pool BUH
Router(dhcp-config)#network 192.168.50.0 255.255.255.0
Router(dhcp-config)#default-router 192.168.50.1
Router(dhcp-config)#
```
## Создание сабинтерфейсов 

```
Router(config)#interface FastEthernet 0/1.10
Router(config-subif)#encapsulation dot1Q 10
Router(config-subif)#ip address 192.168.10.1 255.255.255.0
Router(config-subif)#ex
Router(config)#interface FastEthernet 0/1.20
Router(config-subif)#encapsulation dot1Q 20
Router(config-subif)#ip address 192.168.20.1 255.255.255.0
Router(config-subif)#ex
Router(config)#interface FastEthernet 0/1.30
Router(config-subif)#encapsulation dot1Q 30
Router(config-subif)#ip address 192.168.30.1 255.255.255.0
Router(config-subif)#ex
Router(config)#interface FastEthernet 0/1.40
Router(config-subif)#encapsulation dot1Q 40
Router(config-subif)#ip address 192.168.40.1 255.255.255.0
Router(config-subif)#ex
Router(config)#interface FastEthernet 0/1.50
Router(config-subif)#encapsulation dot1Q 50
Router(config-subif)#ip address 192.168.50.1 255.255.255.0
Router(config-subif)#ex

```
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
# ![images1]()

# !!Настройка DHCP на роутере для wifi сетей была произведена ранее с опцией 43.

# Созадние сетей для Manager и TOP-manager
# ![images2]()

# ![images3]()

# Созадние групп 

# ![images4]()

# ![images5]()

# Создание VPN тоннелей с флиалами 
 # R1-R2
`R1`
```
R1(config)#access-list 110 permit ip 192.168.10.0 0.0.0.255 172.16.1.0 0.0.0.255
R1(config)#access-list 110 permit ip 192.168.20.0 0.0.0.255 172.16.1.0 0.0.0.255
R1(config)#access-list 110 permit ip 192.168.30.0 0.0.0.255 172.16.1.0 0.0.0.255
R1(config)#access-list 110 permit ip 192.168.40.0 0.0.0.255 172.16.1.0 0.0.0.255
R1(config)#access-list 110 permit ip 192.168.50.0 0.0.0.255 172.16.1.0 0.0.0.255
R1(config)#crypto isakmp policy 10
R1(config-isakmp)#encryption 3des 
R1(config-isakmp)#authentication pre-share 
R1(config-isakmp)#hash md5
R1(config-isakmp)#group 2
R1(config-isakmp)#Lifetime 3600
R1(config-isakmp)#ex
R1(config)#crypto isakmp key cisco address 100.0.0.2
R1(config)#crypto ipsec  transform-set VPN-SET ah-md5-hmac esp-3des
R1(config)#crypto map VPN-MAP 10 ipsec-isakmp 
% NOTE: This new crypto map will remain disabled until a peer
        and a valid access list have been configured.
R1(config-crypto-map)#description VPN connection to R2
R1(config-crypto-map)#set peer 100.0.0.2
R1(config-crypto-map)#set transform-set VPN-SET
R1(config-crypto-map)#match address 110
R1(config-crypto-map)#ex
R1(config)#interface gigabitEthernet 0/0/1
R1(config-if)#crypto map VPN-MAP
```
`R2`
```
R2(config)#access-list 110 permit ip 172.16.1.0 0.0.0.255 192.168.10.0 0.0.0.255
R2(config)#access-list 110 permit ip 172.16.1.0 0.0.0.255 192.168.20.0 0.0.0.255
R2(config)#access-list 110 permit ip 172.16.1.0 0.0.0.255 192.168.30.0 0.0.0.255
R2(config)#access-list 110 permit ip 172.16.1.0 0.0.0.255 192.168.40.0 0.0.0.255
R2(config)#access-list 110 permit ip 172.16.1.0 0.0.0.255 192.168.50.0 0.0.0.255
R2(config)#crypto isakmp policy 10
R2(config-isakmp)#encryption 3des 
R2(config-isakmp)#authentication pre-share 
R2(config-isakmp)#hash md5
R2(config-isakmp)#group 2
R2(config-isakmp)#Lifetime 3600
R2(config-isakmp)#ex
R2(config)#crypto isakmp key cisco address 100.0.0.1
R2(config)#crypto ipsec  transform-set VPN-SET ah-md5-hmac esp-3des
R2(config)#crypto map VPN-MAP 10 ipsec-isakmp 
% NOTE: This new crypto map will remain disabled until a peer
        and a valid access list have been configured.
R2(config-crypto-map)#description VPN connection to R1
R2(config-crypto-map)#set peer 100.0.0.1
R2(config-crypto-map)#set transform-set VPN-SET
R2(config-crypto-map)#match address 110
R2(config-crypto-map)#ex
R2(config)#interface gigabitEthernet 0/0/1
R2(config-if)#crypto map VPN-MAP

```


 # R1-R3
`R1`
```
R1(config)#access-list 120 permit ip 192.168.10.0 0.0.0.255 172.20.1.0 0.0.0.255
R1(config)#access-list 120 permit ip 192.168.20.0 0.0.0.255 172.20.1.0 0.0.0.255
R1(config)#access-list 120 permit ip 192.168.30.0 0.0.0.255 172.20.1.0 0.0.0.255
R1(config)#access-list 120 permit ip 192.168.40.0 0.0.0.255 172.20.1.0 0.0.0.255
R1(config)#access-list 120 permit ip 192.168.50.0 0.0.0.255 172.20.1.0 0.0.0.255
R1(config)#crypto isakmp policy 20
R1(config-isakmp)#encryption 3des 
R1(config-isakmp)#authentication pre-share 
R1(config-isakmp)#hash md5
R1(config-isakmp)#group 2
R1(config-isakmp)#Lifetime 3600
R1(config-isakmp)#ex
R1(config)#crypto isakmp key cisco address 100.0.10.2
R1(config)#crypto ipsec  transform-set VPN-SET2 ah-md5-hmac esp-3des
R1(config)#crypto map VPN-MAP2 20 ipsec-isakmp 
% NOTE: This new crypto map will remain disabled until a peer
        and a valid access list have been configured.
R1(config-crypto-map)#description VPN connection to R3
R1(config-crypto-map)#set peer 100.0.10.2
R1(config-crypto-map)#set transform-set VPN-SET2
R1(config-crypto-map)#match address 120
R1(config-crypto-map)#ex
R1(config)#interface FastEthernet 1/0
R1(config-if)#crypto map VPN-MAP2
```
`R3`
```
R3(config)#access-list 110 permit ip 172.20.1.0 0.0.0.255 192.168.10.0 0.0.0.255
R3(config)#access-list 110 permit ip 172.20.1.0 0.0.0.255 192.168.20.0 0.0.0.255
R3(config)#access-list 110 permit ip 172.20.1.0 0.0.0.255 192.168.30.0 0.0.0.255
R3(config)#access-list 110 permit ip 172.20.1.0 0.0.0.255 192.168.40.0 0.0.0.255
R3(config)#access-list 110 permit ip 172.20.1.0 0.0.0.255 192.168.50.0 0.0.0.255
R3(config)#crypto isakmp policy 10
R3(config-isakmp)#encryption 3des 
R3(config-isakmp)#authentication pre-share 
R3(config-isakmp)#hash md5
R3(config-isakmp)#group 2
R3(config-isakmp)#Lifetime 3600
R3(config-isakmp)#ex
R3(config)#crypto isakmp key cisco address 100.0.10.1
R3(config)#crypto ipsec  transform-set VPN-SET ah-md5-hmac esp-3des
R3(config)#crypto map VPN-MAP 10 ipsec-isakmp 
% NOTE: This new crypto map will remain disabled until a peer
        and a valid access list have been configured.
R3(config-crypto-map)#description VPN connection to R1
R3(config-crypto-map)#set peer 100.0.0.1
R3(config-crypto-map)#set transform-set VPN-SET
R3(config-crypto-map)#match address 110
R3(config-crypto-map)#ex
R3(config)#interface gigabitEthernet 0/0/1
R3(config-if)#crypto map VPN-MAP

```
 # R1-R4
`R1`
```
R1(config)#access-list 130 permit ip 192.168.10.0 0.0.0.255 172.30.1.0 0.0.0.255
R1(config)#access-list 130 permit ip 192.168.20.0 0.0.0.255 172.30.1.0 0.0.0.255
R1(config)#access-list 130 permit ip 192.168.30.0 0.0.0.255 172.30.1.0 0.0.0.255
R1(config)#access-list 130 permit ip 192.168.40.0 0.0.0.255 172.30.1.0 0.0.0.255
R1(config)#access-list 130 permit ip 192.168.50.0 0.0.0.255 172.30.1.0 0.0.0.255
R1(config)#crypto isakmp policy 30
R1(config-isakmp)#encryption 3des 
R1(config-isakmp)#authentication pre-share 
R1(config-isakmp)#hash md5
R1(config-isakmp)#group 2
R1(config-isakmp)#Lifetime 3600
R1(config-isakmp)#ex
R1(config)#crypto isakmp key cisco address 100.0.20.2
R1(config)#crypto ipsec  transform-set VPN-SET3 ah-md5-hmac esp-3des
R1(config)#crypto map VPN-MAP3 30 ipsec-isakmp 
% NOTE: This new crypto map will remain disabled until a peer
        and a valid access list have been configured.
R1(config-crypto-map)#description VPN connection to R4
R1(config-crypto-map)#set peer 100.0.20.2
R1(config-crypto-map)#set transform-set VPN-SET3
R1(config-crypto-map)#match address 130
R1(config-crypto-map)#ex
R1(config)#interface FastEthernet 1/1
R1(config-if)#crypto map VPN-MAP3
```
`R4`
```
R4(config)#access-list 110 permit ip 172.30.1.0 0.0.0.255 192.168.10.0 0.0.0.255
R4(config)#access-list 110 permit ip 172.30.1.0 0.0.0.255 192.168.20.0 0.0.0.255
R4(config)#access-list 110 permit ip 172.30.1.0 0.0.0.255 192.168.30.0 0.0.0.255
R4(config)#access-list 110 permit ip 172.30.1.0 0.0.0.255 192.168.40.0 0.0.0.255
R4(config)#access-list 110 permit ip 172.30.1.0 0.0.0.255 192.168.50.0 0.0.0.255
R4(config)#crypto isakmp policy 10
R4(config-isakmp)#encryption 3des 
R4(config-isakmp)#authentication pre-share 
R4(config-isakmp)#hash md5
R4(config-isakmp)#group 2
R4(config-isakmp)#Lifetime 3600
R4(config-isakmp)#ex
R4(config)#crypto isakmp key cisco address 100.0.20.1
R4(config)#crypto ipsec  transform-set VPN-SET ah-md5-hmac esp-3des
R4(config)#crypto map VPN-MAP 10 ipsec-isakmp 
% NOTE: This new crypto map will remain disabled until a peer
        and a valid access list have been configured.
R4(config-crypto-map)#description VPN connection to R1
R4(config-crypto-map)#set peer 100.0.20.1
R4(config-crypto-map)#set transform-set VPN-SET
R4(config-crypto-map)#match address 110
R4(config-crypto-map)#ex
R4(config)#interface gigabitEthernet 0/0/1
R4(config-if)#crypto map VPN-MAP

```

### Правила приема домашнего задания

В личном кабинете отправлены:

- ссылка на документ (Google Doc) с выполненным заданием. В документе настроены права доступа “Просматривать могут все в Интернете, у кого есть ссылка”;
- файл в формате .pkt.

### Критерии оценки

Зачет - выполнены все задания, ответы даны в развернутой форме, приложены соответствующие скриншоты и файлы проекта, в выполненных заданиях нет противоречий и нарушения логики.

На доработку - задание выполнено частично или не выполнено, в логике выполнения заданий есть противоречия, существенные недостатки.
