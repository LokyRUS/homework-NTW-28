# `Основы IPv6 (Базовые понятия, сравнение с IPv4, принципы построения подсетей).`
# Исполнитель: Смирнов Кирилл NTW-28

### Цель задания

1. Научиться отличать пакеты версии IPv6 от IPv4 и работать с ними.
2. Научиться различать типы адресов IPv6.
3. Сконфигурировать сетевой адрес на устройстве с помощью механизма EUI-64.

## Задание 1.

Какого типа трафика нет в IPv6, но который имеется в IPv4? Что предусмотрено взамен?

*Приведите ответ в свободной форме.*

1) В ipv6 отсутствует Broadcast трафик, который заменён на Multicast.
2) Так же, если рассматривать с точки зрения трафика, то в Ipv6 не происходит фрагментация, mtu минимальный размер которого составляет 1280 байт, а во многих сетях используется mtu 1500. Однако, если встретиться сеть с меньшим MTU ,то будет использована технология Path MTU Discovery.Это когда IPv6 пакет можно разбить на меньшие пакеты только на узле отправителя. Сборка пакета может выполняться только на узле получателя.
Однако в ipv4 максимальный MTU равен 576 байт, и в случае фрагментации, если пакет слишком велик для его передачи по каналу связи, узел отправителя может разбить его на несколько фрагментов.
---

### Задание 2.

Используя любую консольную утилиту получите IPv6-адрес для какого либо ресурса.

*Пришлите скриншот.*
# Ответ 

```console
dig google.com AAAA
```
# ![images1](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/1.PNG)
---

### Задание 3.

Какие из этих префиксов содержатся в адресе: 2001:DB8:2314:5678::9ABC:DEF0?

a)2001:DB8:2314:5::/52

b)2001:DB8:2314:5678::9AB8:0/109

c)2001:DB8:2314:5660::/59

d)2001:DB80::/27

# Ответ

`b и с`

---

### Задание 4. Лабораторная работа "Конфигурация сетевых интерфейсов на основе IPv6".

В Cisco Packet Tracer создайте два маршрутизатора (R1 и R2) и настройте между ними адресацию по IPv6 с помощью глобальных адресов.

*Приведите скриншоты, где R2 доступен по ICMPv6 с R1 по ICMPv6*

*Пришлите pkt файл.*

# Ответ

# ![images2](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/2.PNG)

# настройка Router0
```
Router>enable 
Router#configure  terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gigabitEthernet 0/0
Router(config-if)#ipv6 address 2001::123/64
Router(config-if)#no shutdown
Router(config-if)#exit
Router(config)#ipv6 unicast-routing 
Router(config)#
```


# настройка Router1
```
Router>enable 
Router#configure  terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gigabitEthernet 0/0
Router(config-if)#ipv6 address 2001::140/64
Router(config-if)#no shutdown
Router(config-if)#exit
Router(config)#ipv6 unicast-routing 
Router(config)#
```

# Производим динг с Router0 на адрес Router1

```
Router#ping 2001::140
```
# ![images3](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/3.PNG)

# [Файл](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/Zadanie4.pkt)

---

### Дополнительные задания (со звездочкой*)

Эти задания дополнительные (не обязательные к выполнению) и никак не повлияют на получение вами зачета по этому домашнему заданию. Вы можете их выполнить, если хотите глубже и/или шире разобраться в материале.

---
### Задание 5*.

На основе лабораторной работы в Задании 4 к схеме добавьте еще два маршрутизатора R3 к R2, а R4 к R1. Назначьте им IP-адрес-а c помощью механизма EUI-64.

*Приведите скриншот, где отображены настройки интерфейса и ip-адрес*

*Пришлите pkt файл.*

# Ответ

# Настройка Router1 (Gig/01)
```
Router#conf terminal 

Router(config)#interface gigabitEthernet 0/1
Router(config-if)#ipv6 address 2002::/64 eui-64 
Router(config-if)#no shutdown 
Router(config-if)#exit
Router(config)#ipv
Router(config)#ipv6 unicast-routing 
Router(config)#
```
# Настройка Router3 (Gig/01)
```
Router#conf terminal 

Router(config)#interface gigabitEthernet 0/1
Router(config-if)#ipv6 address 2002::/64 eui-64 
Router(config-if)#no shutdown 
Router(config-if)#exit
Router(config)#ipv
Router(config)#ipv6 unicast-routing 
Router(config)#
```

# ![images4]()

# Настройка Router0 (Gig/01)
```
Router#conf terminal 

Router(config)#interface gigabitEthernet 0/1
Router(config-if)#ipv6 address 2002::/64 eui-64 
Router(config-if)#no shutdown 
Router(config-if)#exit
Router(config)#ipv
Router(config)#ipv6 unicast-routing 
Router(config)#
```
# Настройка Router2 (Gig/01)
```
Router#conf terminal 

Router(config)#interface gigabitEthernet 0/1
Router(config-if)#ipv6 address 2002::/64 eui-64 
Router(config-if)#no shutdown 
Router(config-if)#exit
Router(config)#ipv
Router(config)#ipv6 unicast-routing 
Router(config)#
```

# ![images5]()

# [Файл](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/Zadanie5.pkt)

### Задание 6*.

На основе лабораторной работы в Задании 4 настройте на одном из интерфейсов R3 локальный адрес IPv6 и включите DHCPv6. Добавьте к этому интерфейсу ПК/Сервер и сделайте так, чтобы он получил IPv6-адрес.

*Приведите скриншот, где отображены настройки интерфейса и ip-адрес, который получил клиент в этом интерфейсе.*

*Пришлите pkt файл.*

