# `Методы логического объединения интерфейсов. Балансировка нагрузки, протоколы`

# Исполнитель: Смирнов Кирилл NTW-28
### Цель задания

В работе специалиста часто встречаются задачи, требующие обеспечения надежности на узлах сети. Добиться этого можно различными способами, например, резервированием и балансировкой. В данной работе вы попробуете оба. 

В результате выполнения этого задания вы:
1) Научитесь настраивать статическую агрегацию.  
2) Научитесь настраивать динамическую агрегацию с резервированием каналов.  
3) Научитесь настраивать балансировку на L2 уровне.  

------

### Инструкция к выполнению домашнего задания

1. Запустите Cisco Packet Tracer.  
2. Выполните  все задания лабораторной работы.  
3. Проект с выполненным заданием сохраните на диск и прикрепите в личном кабинете в форме отправки домашнего задания.  

------

## Лабораторная работа "Исследование работы логического объединения интерфейсов и балансировки сетевой нагрузки"

### Задание 1

Компания ОАО «XXX – век» планирует увеличить пропускную способность канала связи. К вам поступила заявка: настроить статическую агрегацию для сети.  
Необходимо настроить и проверить доступность компьютеров, проверить настройки через пространство команд show.

![hbc](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/100.PNG)

*Пришлите pkt с полученным проектом*

---
# Ответ

# Настрокай статической агрегации L2 на обоих коммутаторах.
```
Switch>enable 
Switch#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#interface range fastEthernet 0/1-4
Switch(config-if-range)#shutdown 
Switch(config-if-range)#channel-group 1 mode on
Switch(config-if-range)#no shutdown 

```


`Топология`

# ![images1](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/1.PNG)

`Информация об агрегированном канале`


# ![images2](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/2.PNG)

`Сводка о канале`

# ![images3](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/3.PNG)

`Доступность`

# ![images4](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/4.PNG)


# [Скачать-Файл.pkt]()


### Задание 2

Компания ОАО «XXX – век» после модернизации и расширения сети задумалась над резервированием существующего агрегированного канала.    
К вам поступила заявка: настроить динамическую агрегацию каналов для сети, так как есть риск разрыва соединения на основных интерфейсах.   


Необходимо: 
- настроить сетевое оборудование и проверить доступность компьютеров,
- проверить настройки через пространство команд show, 
- проверить работу после отключения основных интерфейсов.

<img width="600" alt="152157053-a276776b-efb9-41e6-9d32-7bbd326373df" src="https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/99.PNG">

*Пришлите pkt с полученным проектом*

---
# Ответ

# отключаем все интрефейсы с fa0/1-8 на каждом из коммутаторов

```
Switch>enable 
Switch#configure terminal 
Switch(config)#interface range fastEthernet 0/1-8
Switch(config-if-range)#shutdown 
```

# Далее назначаем группы 

- `fa0/1-4` - группа 1

- `fa0/5-8` - группа 2

# Насративаем на кадом коммутаторе 

`группа 1`

```
Switch#
Switch#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#interface range fastEthernet 0/1-4
Switch(config-if-range)#channel-group 1 mode active 
Switch(config-if-range)#no shutdown 
```
`группа 2`

```
Switch#
Switch#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#interface range fastEthernet 0/5-8
Switch(config-if-range)#channel-group 2 mode active 
Switch(config-if-range)#no shutdown 
```

`Итоговая рабочая топология`

# ![images5](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/5.PNG)

`Доступность`

# ![images6](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/6.PNG)

`настройки`
# ![images7](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/7.PNG)

# Топология после перекрытия или обрыва линка, доступность

`Этап перестоения`

# ![images8](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/8.PNG)

`'Этап работоспособности`

# ![images9](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/9.PNG)

`Доступность`

# ![images10](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/10.PNG)

# [Скачать-Файл.pkt]()

### Задание 3

Компания ОАО «XXX – век» разместила в своей сети сетевые сервисы. Для увеличения полосы пропускания, повышения надежности и реализации высокой доступности серверов и расположенных на них сервисах Вам поставили задачу доработать топологию сети с учетом возможностей оборудования и настроить балансировку на основе ip адресов источника и получателя.
 
Необходимо: 
- настроить и проверить доступность компьютеров, 
- проверить настройки через пространство команд show.    

**Важно!** В эмуляторе Cisco Packet Tracer есть особенность: часть настроек сетевого оборудования в этой лабораторной работе не сохраняется, когда вы сохраняете ее в .pkt файл.  
**Решение**: На всех сетевых устройствах после окончания настройки нужно выполнить команду "write" или "copy running-config startap-config".

![photo_2023-04-03_13-08-26](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/98.PNG)

<details>
  <summary> Подсказка к заданию 3        
            (постарайтесь выполнить задание, не заглядывая в подсказку) </summary>
 
 Решение: добавьте еще одну связь между коммутатором 2960 и коммутатором 3560, настройте между ними агрегирование, используя L3 EtherChannel на коммутаторе 3560.

</details>

*Пришлите pkt с полученным проектом*

# Ответ

# Топология до агригации каналов

# ![images11](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/11.PNG)

# Настройка агрегации на коммутаторе l2 

```
Switch>enable 
Switch#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#interface range GigabitEthernet 0/1-2
Switch(config-if-range)#shutdown 
Switch(config-if-range)#channel-group 1 mode on
Switch(config-if-range)#no shutdown 
```
# Настройка агрегации на коммутаторе l3

```
Switch>enable 
Switch#configure terminal 
Switch(config)#interface range gigabitEthernet 0/1-2
Switch(config-if-range)#shutdown 
Switch(config-if-range)#channel-group 1 mode on 
Switch(config-if-range)#exit
Switch(config)#interface port-channel 1
Switch(config-if)#no switchport 
Switch(config-if)#ip address 192.168.0.1 255.255.255.0
Switch(config-if)#exit	
Switch(config)#interface range gigabitEthernet 0/1-2
Switch(config-if-range)#no shutdown 

```

# Топология до агригации каналов

# ![images12](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/12.PNG)

`Доступность`

# ![images13](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/13.PNG)

# [Скачать-Файл.pkt]()

### Правила приема домашнего задания

В личном кабинете отправлены pkt-файлы с выполненным проектом.

### Критерии оценки

Зачет - выполнены все задания, ответы даны в развернутой форме, приложены соответствующие скриншоты и файлы проекта, в выполненных заданиях нет противоречий и нарушения логики.

На доработку - задание выполнено частично или не выполнено, в логике выполнения заданий есть противоречия, существенные недостатки.


