# Домашнее задание к занятию "Классификация и маркировка трафика."
## Исполнитель: Смирнов К.Е.-NTW28
### Цель задания

Данное домашнее задание позволит научиться маркировать трафик для дальнейшей приоритезации, а также изучить различные методы классификации.

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

### Лабораторная работа "Классификация и маркировка трафика"

Можете использовать симулятор для построения топологии и поиска/проверки ответов на задания. 

### Задание

На картинке изображена схема сети с тремя маршрутизаторами.

<img width="900" alt="Ping_trouble" src="https://user-images.githubusercontent.com/85602495/169475092-354046d6-f69c-4572-a2aa-4e86f0df9fa6.PNG">

Необходимо классифицировать и маркировать трафик следующим образом:
1) TCP трафик с destination порт 80/443 установить DSCP AF43 между всеми клиентами.
2) UDP трафик с destination порт 5060-5061 установить CS5 между всеми клиентами.
3) Для ICMP трафика значение COS установить 3 между всеми клиентами.
4) Трафик от клиента Client2, Client3 до Client1 должен маркироваться EF.

*Отправьте полный список конфигураций: class-map, policy-map service-policy.*

------
# Ответ:

# R1

## TCP трафик с destination порт 80/443
```
Router(config)#access-list 110 permit tcp 192.168.5.0 0.0.0.255 any eq www 
```
#### class-map
```
Router(config)#class-map match-any USER_TRAFIC_WEB
Router(config-cmap)#match access-group 110
```
## UDP трафик с destination порт 5060-5061
```
Router(config)#access-list 111 permit udp 192.168.5.0 0.0.0.255 any range 5060 5061
```
#### class-map
```
Router(config)#class-map match-any UDP_USER_TRAFIC
Router(config-cmap)#match access-group 111
```
## Трафик ICMP 
```
#access-list 112 permit icmp 192.168.5.0 0.0.0.255 any
```
#### class-map
```
Router(config)#class-map match-any ICMP_USER_TRAFIC
Router(config-cmap)#match access-group 112
```
## policy-map 
```
Router(config)#policy-map TRAFIC_MARK_in
Router(config-pmap)#class USER_TRAFIC_WEB
Router(config-pmap-c)#set ip dscp af43
Router(config-pmap-c)#ex
Router(config-pmap)#class UDP_USER_TRAFIC
Router(config-pmap-c)#set ip dscp cs5
Router(config-pmap)#ex
Router(config-pmap)#class ICMP_USER_TRAFIC
Router(config-pmap-c)#set cos 3
Router(config-pmap-c)#ex
```
### service-policy
```
Router(config)#interface gigabitEthernet 0/0/1
Router(config-if)#service-policy input TRAFIC_MARK_in
```
# R vLOS

## TCP трафик с destination порт 80/443
```
Router(config)#access-list 110 permit tcp 10.10.10.0 0.0.0.255 any eq www 
```
#### class-map
```
Router(config)#class-map match-any USER_TRAFIC_WEB
Router(config-cmap)#match access-group 110
```
## UDP трафик с destination порт 5060-5061
```
Router(config)#access-list 111 permit udp 10.10.10.0 0.0.0.255 any range 5060 5061
```
#### class-map
```
Router(config)#class-map match-any UDP_USER_TRAFIC
Router(config-cmap)#match access-group 111
```
## Трафик ICMP 
```
Router(config)#access-list 112 permit icmp 10.10.10.0 0.0.0.255 any
```
#### class-map
```
Router(config)#class-map match-any ICMP_USER_TRAFIC
Router(config-cmap)#match access-group 112
```
## Трафик от клиента Client3 до Client1 
```
Router(config)#access-list 113 permit ip host 10.10.10.30 host 192.168.5.10
```
#### class-map
```
Router(config)#class-map match-any CLIENT2_CLIENT1
Router(config-cmap)#match access-group 113
```
## policy-map 
```
Router(config)#policy-map TRAFIC_MARK_in
Router(config-pmap)#class USER_TRAFIC_WEB
Router(config-pmap-c)#set ip dscp af43
Router(config-pmap-c)#ex
Router(config-pmap)#class UDP_USER_TRAFIC
Router(config-pmap-c)#set ip dscp cs5
Router(config-pmap)#ex
Router(config-pmap)#class ICMP_USER_TRAFIC
Router(config-pmap-c)#set cos 3
Router(config-pmap-c)#ex

```
### service-policy
```
Router(config)#interface gigabitEthernet 0/0/1
Router(config-if)#service-policy input TRAFIC_MARK_in
```


# R2

## TCP трафик с destination порт 80/443
```
Router(config)#access-list 110 permit tcp 172.16.1.0 0.0.0.255 any eq www 
```
#### class-map
```
Router(config)#class-map match-any USER_TRAFIC_WEB
Router(config-cmap)#match access-group 110
```

## UDP трафик с destination порт 5060-5061
```
Router(config)#access-list 111 permit udp 172.16.1.0 0.0.0.255 any range 5060 5061
```
#### class-map
```
Router(config)#class-map match-any UDP_USER_TRAFIC
Router(config-cmap)#match access-group 111
```

## Трафик ICMP 
```
Router(config)#access-list 112 permit icmp 172.16.1.0 0.0.0.255 any
```
#### class-map
```
Router(config)#class-map match-any ICMP_USER_TRAFIC
Router(config-cmap)#match access-group 112
```

## Трафик от клиента Client2 до Client1 
```

Router(config)#access-list 113 permit ip host 172.16.1.20 host 192.168.5.10
```
#### class-map
```
Router(config)#class-map match-any CLIENT2_CLIENT1
Router(config-cmap)#match access-group 113
```


## policy-map 
```
Router(config)#policy-map TRAFIC_MARK_in
Router(config-pmap)#class USER_TRAFIC_WEB
Router(config-pmap-c)#set ip dscp af43
Router(config-pmap-c)#ex
Router(config-pmap)#class UDP_USER_TRAFIC
Router(config-pmap-c)#set ip dscp cs5
Router(config-pmap)#ex
Router(config-pmap)#class ICMP_USER_TRAFIC
Router(config-pmap-c)#set cos 3
Router(config-pmap-c)#ex
```
### service-policy
```
Router(config)#interface gigabitEthernet 0/0/1
Router(config-if)#service-policy input TRAFIC_MARK_in
```


### Правила приема домашнего задания

В личном кабинете отправлена ссылка на документ (Google Doc) с выполненным заданием. В документе настроены права доступа “Просматривать могут все в Интернете, у кого есть ссылка”.

### Критерии оценки

Зачет - выполнены все задания, ответы даны в развернутой форме, приложены соответствующие скриншоты и файлы проекта, в выполненных заданиях нет противоречий и нарушения логики.

На доработку - задание выполнено частично или не выполнено, в логике выполнения заданий есть противоречия, существенные недостатки.
