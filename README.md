# `Маршрутизация. Шлюз по умолчанию.  Выбор лучшего маршрута.`
# Исполнитель: Смирнов Кирилл NTW-28

### Цель задания

1. Выбрать наилучший маршрут на базе статической маршрутизации.
2. Выбрать наилучший маршрут на базе динамической маршрутизации.
3. Изучить различные параметры выбора маршрута на базе динамической маршрутизации.

### Инструкция к выполнению домашнего задания

### Задание 1. Лабораторная работа "Выбор наилучшего маршрута на базе статической маршрутизации"

Соберите тестовую сеть согласно топологии ниже:

- настройте статическую маршрутизацию для тестовой сети так, чтобы пакеты от PC0 к PC1 маршрутизировались через путь R0 - R2. Проверьте работоспособность сети;
- измените настройки административной дистанции статических маршрутов так, чтобы пакеты от PC0 к PC1 маршрутизировались через путь R0 - R3 - R2, а путь R0 - R2 оставался запасным.
  
Сделайте вывод, как административная дистанция влияет на маршрут.

<img width="737"  src="https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/99-1.PNG">

*Приведите ответ в свободной форме и пришлите pkt-файл.*

# Ответ
# [Первая часть задания - скачать file.pkt](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/zadanie%201%20chast%201%20.pkt)
# [Вторая часть задания - скачать даfile.pkt](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/zadanie%201%20chast%202.pkt)
#	 Настройка интерфейса Gi0/0 на Router0

```
Router>enable 
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gigabitEthernet 0/0
Router(config-if)#ip address 192.168.10.1 255.255.255.0
Router(config-if)#no shutdown 
Router(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up

Router#
%SYS-5-CONFIG_I: Configured from console by console


```
`Пингнуем для проверки`

#	 Настройка интерфейса Gi0/0 на Router1

```
Router>enable 
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gigabitEthernet 0/0
Router(config-if)#ip address 192.168.20.1 255.255.255.0
Router(config-if)#no shutdown 
Router(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up

Router#
%SYS-5-CONFIG_I: Configured from console by console

```
`Пингуем для проверки`

# Настроим соединение между Router0 (Gig0/1- ip 10.1.1.1/30)и Router2 (Gig0/1- ip 10.1.1.2/30)

### Настройка Router0 
```
Router#
Router#enable 
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gigabitEthernet 0/1
Router(config-if)#ip address 10.1.1.1 255.255.255.252
Router(config-if)#no shutdown 
Router(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/1, changed state to up
```

### Настройка Router2
```
Router>enable 
Router#configure ter
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gigabitEthernet 0/1
Router(config-if)#ip address 10.1.1.2 255.255.255.252
Router(config-if)#no shutdown 
Router(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/1, changed state to up

```
`Пингуем для проверки`

# Настроим соединение между Router1 (Gig0/2- ip 10.1.2.1/30)и Router2 (Gig0/2- ip 10.1.2.2/30)

### Настройка Router1 
```
Router#
Router#enable 
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gigabitEthernet 0/2
Router(config-if)#ip address 10.1.2.1 255.255.255.252
Router(config-if)#no shutdown 
Router(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/2, changed state to up
```

### Настройка Router2
```
Router>enable 
Router#configure ter
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gigabitEthernet 0/2
Router(config-if)#ip address 10.1.2.2 255.255.255.252
Router(config-if)#no shutdown 
Router(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/2, changed state to up

```
`Пингуем для проверки`

# Настроим соединение между Router0 (Gig0/2- ip 10.1.3.1/30)и Router1 (Gig0/1- ip 10.1.3.2/30)

### Настройка Router0 
```
Router#
Router#enable 
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gigabitEthernet 0/2
Router(config-if)#ip address 10.1.3.1 255.255.255.252
Router(config-if)#no shutdown 
Router(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/2, changed state to up
```

### Настройка Router1
```
Router>enable 
Router#configure ter
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gigabitEthernet 0/2
Router(config-if)#ip address 10.1.3.2 255.255.255.252
Router(config-if)#no shutdown 
Router(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/2, changed state to up

```
`Пингуем для проверки`

# Первая часть задания маршрутизация через нижний путь Router0 и Router1.

### Настройка Router0 
```
Router#
Router#en
Router#enable 
Router#configure ter
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#ip route 192.168.20.0 255.255.255.0 10.1.3.2
Router(config)#exit
Router#
%SYS-5-CONFIG_I: Configured from console by console

```

### Настройка Router1
```
Router#
Router#en
Router#enable 
Router#configure ter
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#ip route 192.168.10.0 255.255.255.0 10.1.3.1
Router(config)#exit
Router#
%SYS-5-CONFIG_I: Configured from console by console

```
`Пингуем и делаем трасировку для проверки`

# ![images1](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/1.PNG)

# ![images2](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/2.PNG)

[Файл.pkt] ()

# Вторая часть задания маршрутизация через  верхний путь Router0-Router2-Router1, нижний путь Router0 и Router1 остается запасным.

`Основной путь`

### Настройка Router0 
```
Router#
Router#en
Router#enable 
Router#configure ter
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#ip route 192.168.20.0 255.255.255.0 10.1.1.2
Router(config)#exit
Router#
%SYS-5-CONFIG_I: Configured from console by console

```
### Настройка Router1 
```
Router#
Router#en
Router#enable 
Router#configure ter
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#ip route 192.168.10.0 255.255.255.0 10.1.2.2
Router(config)#exit
Router#
%SYS-5-CONFIG_I: Configured from console by console
```

### Настройка Router2
```
Router#
Router#en
Router#enable 
Router#configure ter
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#ip route 192.168.10.0 255.255.255.0 10.1.1.1
Router(config)#ip route 192.168.20.0 255.255.255.0 10.1.2.1
Router(config)#exit
Router#
%SYS-5-CONFIG_I: Configured from console by console
``` 
`Пингуем и делаем трасировку для проверки`

# ![images3](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/3.PNG)

# ![images4](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/4.PNG)

`Резервный путь`

### Настройка Router0 
```
Router#
Router#en
Router#enable 
Router#configure ter
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#ip route 192.168.20.0 255.255.255.0 10.1.3.2 210
Router(config)#exit
Router#
%SYS-5-CONFIG_I: Configured from console by console

```

### Настройка Router1
```
Router#
Router#en
Router#enable 
Router#configure ter
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#ip route 192.168.10.0 255.255.255.0 10.1.3.1 210
Router(config)#exit
Router#
%SYS-5-CONFIG_I: Configured from console by console

```

`проверка`
Для корректной работы следует отключить основную линию связи полностью, иначе пинг может не вернуться, а пойти по работоспособному куску главной линии. 
На скриншоте видно как поменялся маршрут. В этом минус статической маршрутизации при использовании в кольцевой топологии.
# ![images5](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/5.PNG)
# ![images6](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/6.PNG)

[Файл.pkt] ()



### Задание 2. Лабораторная работа "Выбор наилучшего маршрута на базе динамической маршрутизации"

Соберите тестовую сеть, настройте протокол маршрутизации rip для тестовой сети согласно топологии. 

-	Проверьте работоспособность сети. 
-	Подключите R6 и R7 маршрутизаторы в разрыв линии R2-R4 маршрутизаторов. Вместо соединения R2-R4 должно быть R2-R6-R7-R4.
-	Выполните их настройку. 
-	Протестируйте полученную сеть. 

Как подключение маршрутизаторов повлияло на метрики выбора маршрута и почему, какие выводы вы можете сделать до и после подключения.

<img width="737"  src="https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/99-2.PNG">
*Приведите ответ в свободной форме и пришлите pkt файл.*

---

### Правила приема домашнего задания

В личном кабинете отправленs:
- ссылка на документ (Google Doc) с выполненным заданием. В документе настроены права доступа “Просматривать могут все в Интернете, у кого есть ссылка”;  
- pkt файл.

### Критерии оценки

Зачет - выполнены все задания, ответы даны в развернутой форме, приложены соответствующие скриншоты и файлы проекта, в выполненных заданиях нет противоречий и нарушения логики.

На доработку - задание выполнено частично или не выполнено, в логике выполнения заданий есть противоречия, существенные недостатки.



