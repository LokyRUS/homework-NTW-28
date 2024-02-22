# `Принципы коммутации. VLAN.`
# Исполнитель: Смирнов Кирилл NTW-28

### Цель задания
Научиться создавать и настраивать VLAN.


### Лабораторная работа "Создание VLAN"
### Задание 1. 

В программе Cisco PacketTracer составьте сеть из двух коммутаторов и компьютеров.

Создайте три VLAN так, чтобы:

- в каждой из них находилось минимум два компьютера;
- хотя бы одна VLAN была на разных коммутаторах.

Проверьте связь между хостами командой ping.

*Приведите ответ в виде pkt-файла.*

------
# Ответ:

### Созданная топология
- swich - 2шт. 
- host - 6 шт.

Пулл ip для хостов  `192.168.0.10` - `192.168.0.15`

  |VLAN |IP|
| --- | --- |
| VLAN 10 | {192.168.0.10; 192.168.0.13 } |
| VLAN 20 | {192.168.0.11; 192.168.0.12 } |
| VLAN 30 | {192.168.0.14; 192.168.0.15 } |

# ![images1]()

# ! Проверим связанность хостов для заполнения твблицы мак адресов  

# Настройка VLAN 10

# !В данном случае требуется настроить  vlan10 , для взаимодействия, на одобих коммутаторах. 

### Настройка vlan 10 на swich 1
-`создаем vlan10 и переведем интерфейс Fa0/1, на коммутаторе, в режим access vlan 10 
```
Switch>enable 

Switch#configure terminal 
Switch(config)#vlan 10
Switch(config-vlan)#name vlan10
Switch(config-vlan)#exit
Switch(config)#interface fastEthernet 0/1
Switch(config-if)#switchport mode access 
Switch(config-if)#switchport access vlan 10
Switch(config-if)#exit
Switch(config)#exit
```

- `Проверка настроек `

# ![images2]()

### Настройка vlan 10 на swich 2
-`создаем vlan10 и переведем интерфейс Fa0/1, на коммутаторе, в режим access vlan 10 
```
Switch2>enable 

Switch2#configure terminal 
Switch2(config)#vlan 10
Switch2(config-vlan)#name vlan10
Switch2(config-vlan)#exit
Switch2(config)#interface fastEthernet 0/1
Switch2(config-if)#switchport mode access 
Switch2(config-if)#switchport access vlan 10
Switch2(config-if)#exit
Switch2(config)#exit
```

- `Проверка настроек 
# ![images3]()

#! Переведем линк между `Switch` и `Switch2` в trunk режим, без данного режима не будет связанности Vlan10, так как они хазодятся за разными коммутаторами.

### Настройка mode trunk на swich
- перведем interface Fa0/24 в mode trunk

```
Switch>enable 
Switch#configure terminal 
Switch(config)#in
Switch(config)#interface fastEthernet 0/24
Switch(config-if)#switchport mode trunk 

Switch(config-if)#
%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/24, changed state to down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/24, changed state to up
```

### Настройка mode trunk на Switch 2
- перведем interface Fa0/24 в mode trunk
```
Switch2>enable 
Switch2#configure terminal 
Switch(config)#in
Switch2(config)#interface fastEthernet 0/24
Switch2(config-if)#switchport mode trunk 

Switch2(config-if)#
%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/24, changed state to down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/24, changed state to up
```

## проверка связанности

# ![imges4]()

### Настройка vlan 20 на swich 1
-`создаем vlan20 и переведем интерфейсы Fa0/2 и Fa0/3, на коммутаторе, в режим access vlan 20 
```
Switch>enable 

Switch#configure terminal 
Switch(config)#vlan 20
Switch(config-vlan)#name vlan20
Switch(config-vlan)#exit
Switch(config)#interface fastEthernet 0/2
Switch(config-if)#switchport mode access 
Switch(config-if)#switchport access vlan 20
Switch(config-if)#exit
Switch(config)#interface fastEthernet 0/3
Switch(config-if)#switchport mode access 
Switch(config-if)#switchport access vlan 20
Switch(config-if)#exit
Switch(config)#exit
```
## проверка связанности
`! Видно что отсутствует связанность с vlan10`

# ![imges5]()

### Настройка vlan 30 на swich 2
-`создаем vlan30 и переведем интерфейсы Fa0/2 и Fa0/3, на коммутаторе, в режим access vlan 30 
```
Switch2>enable 

Switch2#configure terminal 
Switch2(config)#vlan 30
Switch2(config-vlan)#name vlan30
Switch2(config-vlan)#exit
Switch2(config)#interface fastEthernet 0/2
Switch2(config-if)#switchport mode access 
Switch2(config-if)#switchport access vlan 30
Switch2(config-if)#exit
Switch2(config)#interface fastEthernet 0/3
Switch2(config-if)#switchport mode access 
Switch2(config-if)#switchport access vlan 30
Switch2(config-if)#exit
Switch2(config)#exit
```
## проверка связанности
`! Видно что отсутствует связанность с vlan10 и vlan20`

# ![imges6]()

# [скачать.ptk] ()


## Дополнительные задания (со звездочкой*)
Эти задания дополнительные (не обязательные к выполнению) и никак не повлияют на получение вами зачета по этому домашнему заданию. Вы можете их выполнить, если хотите глубже и/или шире разобраться в материале.

### Задание 2*. 

1. Настройте на каждом коммутаторе по одному дополнительному порту в **Vlan 10** (см. примечание) и введите команду **no spanning-tree vlan 10**.
2. Соедините эти два порта с помощью дополнительного кабеля.
3. Проверьте связь с помощью ping внутри этой VLAN в коммутаторе и в соседних VLAN.
  
Примечание: если в первой части ДЗ вы не использовали vlan 10, то можете взять любой существующий номер, но в итоге у вас должно получиться два trunk в которых разрешен один и тот же vlan.

Вопрос: есть ли разница? Если да, то в чём?

*Приведите ответ в свободной форме.*

