# `Протоколы FHRP`

# Исполнитель: Смирнов Кирилл NTW-28
### Цель задания
В современном мире работа большинства компаний зависит от надежной связи между филиалами компании и стабильным доступом в сеть Интернет.
Помимо наличия резервных каналов необходимо чтобы переключение между ними происходило быстро и максимально незаметно для рядовых сотрудников.
Протоколы семейства FHRP разработаны для решения таких задач. Знание этих протоколов, опыт их настройки, эксплуатации и диагностики возможных проблем, очень важны в работе сетевого администратора.

В результате выполнения этого задания вы научитесь выбирать и настраивать протоколы семейства FHRP.

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

------

### Кейс

У вашей компании есть центральный офис и филиал. В центральном офисе коммутатор и два маршрутизатора. В филиале один коммутатор и один маршрутизатор.
До филиала построено два канала L3. Один канал основной и второй канал - резервный. 
Резервный канал - дорогой, оплата по количеству трафика, пользоваться им необходимо по возможности реже. 
При этом необходимо автоматическое переключение в случае отказа основного канала и возврат в исходное состояние при восстановлении связи.
Сеть офиса - 192.168.0.0/24
Сеть филиала - 192.168.1.0/24
Основной канал - 10.0.1.0/30
Резервный канал - 10.0.2.0/30

### Задание 1

Уточнение - оборудование __Juniper__

1. Выбрать протокол из семейтсва FHRP и обосновать свой выбор.
2. Нарисовать схему сети.
3. Настроить маршрутизаторы центрального офиса в соответствии с выбранным протоколом.

*Ответы на вопрос 1 привести в свободной форме, в текстовом виде.  
 На вопрос 2 - приложить файл в формате .png или .jpg.  
 На вопрос 3 - привести настройки интерфейсов в текстовом виде (в синтаксисе Juniper - [ссылка на руководство](https://www.juniper.net/documentation/ru/ru/software/junos/high-availability/topics/example/vrrp-configuring-example.html))*

---
# Ответ 

## 1) выбор пал на VRRP, он поддерживается juniper. HSRP и GLBP -Проприетарный протокол Cisco 

## 2)---

# ![images1](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/1.PNG)

## 3)---

# Настройка Router 0

```
user@Router0# set interfaces ge-0/0/2 unit 0 family inet address 192.168.0.1/24
user@router0# set interfaces ge-0/0/1 unit 0 family inet address 10.0.1.1/24
```
```
user@router0# set vrrp-group 1 virtual-address 192.168.0.254
```
```
user@router0# set vrrp-group 1 accept-data
```
```
set routing-options static route 192.168.1.0/24 next-hop 10.1.1.2
```

# Настройка Router 1

```
user@Router1# set interfaces ge-0/0/2 unit 0 family inet address 192.168.0.2/24
user@Router1# set interfaces ge-0/0/1 unit 0 family inet address 10.0.2.1/24
```
```
user@router1# set vrrp-group 1 virtual-address 192.168.0.254
```
```
user@router1# set vrrp-group 1 priority 110
```
```
user@router1# set vrrp-group 1 track interface ge-0/0/2 priority-cost 20
```
```
user@router0# set vrrp-group 1 accept-data
```
```
set routing-options static route 192.168.1.0/24 next-hop 10.1.2.2
```
# Настройка Router 2
```
user@Router2# set interfaces ge-0/0/1 unit 0 family inet address 10.0.1.2/24
user@Router2# set interfaces ge-0/0/2 unit 0 family inet address 10.0.2.2/24
```

```
set routing-options static route 192.168.0.0/24 next-hop 10.1.1.1 preference 6
set routing-options static route 192.168.0.0/24 next-hop 10.1.2.1 
```

### `preference` - аналог административной дистанции маршрута в cisco. Изначально, для статического маршрута значение preference = 5. Ставлю для резервного маршрута preference = 6, чтобы использоват  в случае недоступности основого next-hop адреса.


### Задание 2. Лабораторная работа "Выбор и настройка протокола из семейства FHRP"

Компания из кейса, но все оборудование __Cisco__. 

1. Выбрать протокол из семейства FHRP и обосновать свой выбор.
2. Построить топологию в Сisco Packet Tracer. 
3. Настроить оборудование центрального офиса и филиала.
4. Проверить работу резервирования связи с филиалом. Для этого отключить основной канал, проверить командами ping и tracert доступность ПК в филиале и путь до него.

*На вопрос 1 - ответ в свободной форме, в текстовом виде.  
На вопросы 2 и 3 - приложить .pkt файл.  
На вопрос 4 - скриншоты выполнения команд ping и tracert.*

---

## 1)Выбор пал на HSRP, является проприетарный протокол Cisco и балансировка трафика не требуется. 

## 2)---

# ![images2](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/2.PNG)

###3)--- 
# [Скачать File.pkt](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/%D0%B7%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%202.pkt) 

# Настройка Router 1 
```
Router>enable 
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gigabitEthernet 0/1
Router(config-if)#ip address 192.168.0.1 255.255.255.0
Router(config-if)#no shutdown 
Router(config-if)#ex

Router(config)#interface gigabitEthernet 0/0
Router(config-if)#ip address 10.0.1.1 255.255.255.252
Router(config-if)#no shutdown 
Router(config-if)#ex

```
# Настройка Router 2 
```
Router>enable 
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gigabitEthernet 0/1
Router(config-if)#ip address 192.168.0.2 255.255.255.0
Router(config-if)#no shutdown 
Router(config-if)#ex

Router(config)#interface gigabitEthernet 0/0
Router(config-if)#ip address 10.0.2.1 255.255.255.252
Router(config-if)#no shutdown 
Router(config-if)#ex

```
# Настройка HSRP на Router 1 
```
Router(config)#interface gigabitEthernet 0/1
Router(config-if)#standby 1 ip 192.168.0.1
Router(config-if)#standby 1  priority 100

```

# Настройка HSRP на Router 2 c отслеживанием интерфейса gigabitEthernet 0/2
```
Router(config)#interface gigabitEthernet 0/1
Router(config-if)#standby 1 ip 192.168.0.1
Router(config-if)#standby 1  priority 95
Router(config-if)#standby 1 preemp
Router(config-if)standby 1 track gigabitEthernet 0/2 
```

# Настройка Router 3 
```
Router>enable 
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gigabitEthernet 0/1
Router(config-if)#ip address 192.168.1.1 255.255.255.0
Router(config-if)#no shutdown 
Router(config-if)#ex

Router(config)#interface gigabitEthernet 0/0
Router(config-if)#ip address 10.0.1.2 255.255.255.252
Router(config-if)#no shutdown 
Router(config-if)#ex

Router(config)#interface gigabitEthernet 0/0
Router(config-if)#ip address 10.0.2.2 255.255.255.252
Router(config-if)#no shutdown 
Router(config-if)#ex
```
# Настройка маршрутов 

# Настройка Router 1
```
Router(config)#ip route 192.168.1.0 255.255.255.0 10.0.1.2
```
# Настройка Router 2
```
Router(config)#ip route 192.168.1.0 255.255.255.0 10.0.2.2
```

# Настройка Router 3 c приоритетом маршрутов
```
Router(config)#ip route 192.168.0.0 255.255.255.0 10.0.2.1 10
Router(config)#ip route 192.168.0.0 255.255.255.0 10.0.1.1 20
```


### Задание 3

Компания та же, но в центральном офисе добавили еще один маршрутизатор. Оборудование __Cisco__.
Адресация для каналов Интернет:
1. 10.1.0.0/30
2. 10.2.0.0/30
3. 10.3.0.0/30

Для имитации сети Интернет можно добавить еще один маршрутизатор, к которому подключить все три канала интернет. 
На этом же маршрутизаторе создать Loopback интерфейс с адресом 10.10.10.10/32 и использовать этот адрес для проверки доступности сети Интернет.

Необходимо обеспечить компании доступ в Интернет. Непрерывно и с балансировкой по трем каналам.

1. Выбрать протокол из семейства FHRP и обосновать свой выбор.
2. Нарисовать схему сети.
3. Настроить маршрутизаторы центрального офиса в соответствии с выбранным протоколом.

*Ответы на вопрос 1 привести в свободной форме, в текстовом виде.  
На вопрос 2 - приложить файл в формате .png или .jpg.  
На вопрос 3 - привести настройки интерфейсов в текстовом виде (в синтаксисе Cisco)*

# Ответ 


## 1)Выбор GLBP , является проприетарный протокол Cisco? который так же может балансировать трафик. 

## 2)--- 

# ![images3]()

# 3)---
# Общие натсройки связанности

# Настройка Router 1 

```
Router>enable 
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gigabitEthernet 0/0
Router(config-if)#ip address 10.1.0.1 255.255.255.252
Router(config-if)#no shutdown 
```
# Настройка Router 0 

```
Router>enable 
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gigabitEthernet 0/2
Router(config-if)#ip address 10.2.0.1 255.255.255.252
Router(config-if)#no shutdown 
```

# Настройка Router 4 

```
Router>enable 
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gigabitEthernet 0/1
Router(config-if)#ip address 10.3.0.1 255.255.255.252
Router(config-if)#no shutdown 
Router(config-if)#ex
Router(config)#interface gigabitEthernet 0/0
Router(config-if)#ip address 192.168.0.3 255.255.255.0
```

# Настройка Router 3 Провайдер 


# настройка loopback
```
Router(config)#interface loopback 1
Router(config-if)#ip ad 8.8.8.8 255.255.255.255
```
# Общие натсройки связанности
```
Router>enable 
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gigabitEthernet 0/0
Router(config-if)#ip address 10.1.0.2 255.255.255.252
Router(config-if)#no shutdown 
Router(config-if)#ex
Router(config)#interface gigabitEthernet 0/1
Router(config-if)#ip address 10.2.0.2 255.255.255.252
Router(config-if)#ex
Router(config)#interface gigabitEthernet 0/2
Router(config-if)#ip address 10.3.0.2 255.255.255.252
```
# Настройк GLBP

## Настройка Router 1 
```
Router(config-if)#glbp 1 ip 192.168.0.1
Router(config-if)#glbp 1 priority 255
Router(config-if)#glbp 1 preempt delay minimum 15
Router(config-if)#glbp 1 authentication md5 XXXXXX
```
## Настройка Router 0 
```
Router(config-if)#glbp 1 ip 192.168.0.1
Router(config-if)#glbp 1 preempt delay minimum 15
Router(config-if)#glbp 1 authentication md5 XXXXXX
```
## Настройка Router 4 
```
Router(config-if)#glbp 1 ip 192.168.0.1
Router(config-if)#glbp 1 priority 90
Router(config-if)#glbp 1 authentication md5 XXXXXX
```

### Правила приема домашнего задания

В личном кабинете отправлены:

- ссылка на документ (Google Doc) с выполненным заданием. В документе настроены права доступа “Просматривать могут все в Интернете, у кого есть ссылка”;
- pkt-файл с выполненным проектом;
- файл в формате .png или .jpg.

### Критерии оценки

Зачет - выполнены все задания, ответы даны в развернутой форме, приложены соответствующие скриншоты и файлы проекта, в выполненных заданиях нет противоречий и нарушения логики.

На доработку - задание выполнено частично или не выполнено, в логике выполнения заданий есть противоречия, существенные недостатки.
