# Домашнее задание к занятию "Методы контроля и управления доступом к сети"

### Цель задания

Данное домашнее задание научит вас конфигурировать Port-Security и RBAC , а также понимать, какие решения необходимо использовать для различных задач по улучшению информационной безопасности.

В результате выполнения этого задания вы:

1) Научитесь конфигурировать Port-Security
2) Научитесь настраивать RBAC политики

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
### Лабораторная работа "Настройка Port-Security"

Рекомендуем использовать симулятор для построения топологии и проверки/поиска ответов на задания. 

### Задание 1. 

На картинке изображена схема подключения хаба к свитчу. К хабу одновременно может подключаться не более 3 трех устройств. Если будет обнаружено, что подключено более 3 устройств, то необходимо запретить трафик с дополнительных устройств и отправить уведомление о нарушении. 


![image](https://user-images.githubusercontent.com/51816695/155541965-c60aa0fe-ebdb-465f-adcd-9d6bcecd759c.png)

Как необходимо настроить порт Fa0/5 на свитче?

*Отправьте список команд.*
# Ответ

# [Файл для скачивания]()

# ![images1](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/1.PNG)
### Cозданная тополопогия 

|Легитимные хосты|ip|mac address|port хаба|
| --- | --- | --- |---|
| PC1 | 192.168.0.10 | 000C.CF30.5A7A |Fa1|
| PC2 | 192.168.0.20 | 000C.CF84.60C0 |Fa2|
| PC3 | 192.168.0.30 | 0060.3E03.629B |Fah3|

|Hелегитимный хост|ip|mac address|
| --- | --- | --- |
| PC4 | 192.168.0.40 | 0009.7CE9.E05C|

|Сетевое оборудование|port|
| --- |---|
| swich|Fa0/1|
|Hub|Fa0|


# Настройка Port `security`

```
Switch#conf terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#interface fastEthernet 0/1
Switch(config-if)#switchport port-security 
Command rejected: FastEthernet0/1 is a dynamic port.
Switch(config-if)#switchport mode access 
Switch(config-if)#switchport port-security 
Switch(config-if)#switchport port-security mac-address sticky 
Switch(config-if)#switchport port-security maximum 3
Switch(config-if)#switchport port-security aging time 20
Switch(config-if)#switchport port-security violation restrict 
Switch(config-if)#ex
Switch(config)#ex
Switch#
%SYS-5-CONFIG_I: Configured from console by console
```
Настройка `vlan 1` интерфейса для `ping`
```
Switch(config)#interface vlan 1
Switch(config-if)#ip address 192.168.0.1 255.255.255.0
Switch(config-if)#no shutdown 
```
# проверка Работоспособности 
`Этап1`
`Произведем `ping`Легитимными хостами`

`PC1-192.168.0.10`
# ![images2](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/2.PNG)

`PC2-192.168.0.20`
# ![images3](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/3.PNG)

`PC3-192.168.0.30`
# ![images4](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/4.PNG)
## подключим нелигитиный зост  и произведем `ping`
`PC3-192.168.0.40` - `пинг блокируется и пакеты отбраываются`
# ![images5](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/5.PNG)

## В mac-таблице присутствуют только легитимные мак-адреса 
# ![images6](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/6.PNG)

### Задание 2. 

Для отдела мониторинга необходимо настроить view monitoring, в котором можно будет делать следующие операции:
- Смотреть логи устройства
- Смотреть таблицу MAC адресов
- Смотреть таблицу arp
- Смотреть полную конфигурацию свитча

*Отправьте конфигурацию parser view monitoring.*

# Ответ

```
Enter configuration commands, one per line.  End with CNTL/Z.Router(config)#
Router(config)#aaa new-model
Router(config)#enable secret cisco
Router(config)#ex
Router#
%SYS-5-CONFIG_I: Configured from console by console
Router#enable view
Password: cisco
Router#%PARSER-6-VIEW_SWITCH: successfully set to view 'root'.
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#parser view TEST
Router(config-view)#%PARSER-6-VIEW_CREATED: view 'TEST' successfully created.
Router(config-view)#secret cisco
Router(config-view)#commands exec include show arp
Router(config-view)#commands exec include show mac-address-table
Router(config-view)#commands exec include show logging
Router(config-view)#commands exec include show running-config
Router(config-view)#ex
Router(config)#ex

Router#enable view
Router#enable view TEST
Password: Cisco
Router#%PARSER-6-VIEW_SWITCH: successfully set to view 'TEST'.
```
# ![images7](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/7.PNG)

# [Файл для скачивания]()

### Правила приема домашнего задания

В личном кабинете отправлена ссылка на документ (Google Doc) с выполненным заданием. В документе настроены права доступа “Просматривать могут все в Интернете, у кого есть ссылка”.

### Критерии оценки

Зачет - выполнены все задания, ответы даны в развернутой форме, приложены соответствующие скриншоты и файлы проекта, в выполненных заданиях нет противоречий и нарушения логики.

На доработку - задание выполнено частично или не выполнено, в логике выполнения заданий есть противоречия, существенные недостатки.
