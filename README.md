# Домашнее задание к занятию "Сбор и учет данных (NetFlow, syslog)"
# Исполнитель: Смирнов К.Е.

### Цель задания

В результате выполнения задания вы научитесь:  

1. Настраивать отправку логов с оборудования
2. Настраивать отправку статистики NetFlow с оборудования
3. Анализировать NetFlow статистику

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
![1](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/99.PNG)

### Задание 1. 

Сконфигурировать NetFlow на маршрутизаторе для отправки данных на сервер. 

NetFlow должен собирать следующие параметры из трафика: 
- Source/Destination IP
- ToS byte, tcp flags
- next-hop 

IP адресация произвольная. 

Запустите пинги и телнеты на разные порты между двумя компьютерами.

*Пришлите конфигурацию маршрутизатора и скрины NetFlow коллектора*

------
# Ответ

## Настройк интерфейсов на роутере

```
Router>en
Router>enable 
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gigabitEthernet 0/1
Router(config-if)#ip address 192.168.10.254 255.255.255.0
Router(config-if)#no shutdown 

Router(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to up

Router(config)#interface gigabitEthernet 0/0
Router(config-if)#ip address 10.10.10.254 255.255.255.0
Router(config-if)#no shutdown 

Router(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up
```

# Настройкка Netflow
## Создаем RECORD и первый параметр для сбора нофрмации `Source/Destination IP` `tos` `tcp flags` и `next-hop`

```
Router(config)#flow record RECORD
Router(config-flow-record)#match ipv4 source address 
Router(config-flow-record)#match ipv4 destination address 
Router(config-flow-record)#match ipv4 tos 
Router(config-flow-record)#collect transport tcp flags
Router(config-flow-record)#collect routing next-hop address ipv4
Router(config-flow-record)#
```

# Настройка экспортера 

```
Router(config)#flow exporter EXPORTER
Router(config-flow-exporter)#destination 192.168.10.100
Router(config-flow-exporter)#transport udp 9996
Router(config-flow-exporter)#
```
# Настройка монитор

```
Router(config)#flow monitor MONITOR
Router(config-flow-monitor)# exporter EXPORTER
Router(config-flow-monitor)# record RECORD
```

# Вешаем на интерфейс

```
Router(config)#interface gigabitEthernet 0/0
Router(config-if)#ip flow monitor MONITOR input 
Router(config-if)#ip flow monitor MONITOR output 
Router(config-if)#
```
# ![images1](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/1.PNG)
# ![images2](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/2.PNG)

### Задание 2. 

Сконфигурировать Syslog на маршрутизаторе для отправки данных на сервер.  

Выключите gi0/1 на интерфейсе маршрутизатора и получите syslog сообщение на сервере.

*Пришлите конфигурацию маршрутизатора и скрины полученных логов*

------

# Ответ

## Включаем на сервере syslog
 
 # ![images3](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/3.PNG)
 
# Включаем логирование на роутере 
```
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#login on
Router(config)#logging host 192.168.10.100
```
### Выключаем интерфейс и смотрим логи gi0/0 ( Внешний ) 

 # ![images4](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/4.PNG)
```
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#login on
Router(config)#logging host 192.168.10.100
```
### Выключаем интерфейс и смотрим логи gi0/0 ( Внешний ) 

 # ![images4]()

### Правила приема домашнего задания

В личном кабинете отправлены:

- ссылка на документ (Google Doc) с выполненным заданием. В документе настроены права доступа “Просматривать могут все в Интернете, у кого есть ссылка”;
- файл в формате .png или .jpg.


### Критерии оценки

Зачет - выполнены все задания, ответы даны в развернутой форме, приложены соответствующие скриншоты и файлы проекта, в выполненных заданиях нет противоречий и нарушения логики.

На доработку - задание выполнено частично или не выполнено, в логике выполнения заданий есть противоречия, существенные недостатки.
