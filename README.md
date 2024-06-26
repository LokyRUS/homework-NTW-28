# Домашнее задание к занятию "BGP в корпоративных сетях. Построение соседства, машина состояний, типы сообщений"
# Исполнитель: Смирнов Кирилл NTW-28
### Цель задания
Данное домашнее задание позволит научиться выполнять базовую настройку протокола BGP, что является важным навыком для сетевого администратора. 
Также вы научитесь конфигурировать передачу маршрутной информации и направление трафика. Конфигурация и понимание основных принципов - это фундаментальные навыки, которые необходимы при работе с BGP. 

В результате выполенения этого задания вы:
1. Поймете, как происходит обмен маршрутами BGP
2. Научитесь настраивать BGP между маршрутизаторами
3. Освоите базовую настройку маршрутизации BGP

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

### Задание 1

Существует установленная BGP-сессия, у которой uptime 20 часов. Какие сообщения гарантированно не передавались между соседями последние 2 часа? 

*Приведите ответ в свободной форме.*

# Ответ: 
`Keepalive` – по умолчанию каждые 60 секунд.
 Ещё, возможно, `Route refresh`


### Задание 2. Лабораторная работа "Настройка конфигурации BGP"

1. В Cisco Packet Tracer соберите сеть, состоящую из двух маршрутизаторов R1 и R2, находящиеся в разных AS.
2. Настройте между ними BGP.
3. К маршрутизатору R1 добавьте еще маршрутизатор R5, а к R2 - маршрутизатор R4.
4. Соедините R4 и R5 между собой и настройте BGP. 
5. Все маршрутизаторы должны находиться в уникальных AS. 

*Пришлите pkt файл.*

# Ответ

# [Скачать Файл.pkt](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/dz.pkt)
# ![iamges1](https://github.com/LokyRUS/homework-NTW-28/blob/nevidimka/images/1.PNG)

`Настройка R1`
```
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#router bgp 100
Router(config-router)#network 192.168.10.0 mask 255.255.255.0
Router(config-router)#network 10.10.0.0 mask 255.255.255.252
Router(config-router)#network 10.20.0.0 mask 255.255.255.252
Router(config-router)#neighbor 10.10.0.2 remote-as 200
Router(config-router)#neighbor 10.10.20.1 remote-as 500
```
`Настройка R2`
```
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#router bgp 1200
Router(config-router)#network 192.168.20.0 mask 255.255.255.0
Router(config-router)#network 10.10.0.0 mask 255.255.255.252
Router(config-router)#network 10.30.0.0 mask 255.255.255.252
Router(config-router)#neighbor 10.10.0.1 remote-as 100
Router(config-router)#neighbor 10.10.30.1 remote-as 400
```
`Настройка R4`
```
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#router bgp 400
Router(config-router)#network 192.168.30.0 mask 255.255.255.0
Router(config-router)#network 10.10.30.0 mask 255.255.255.252
Router(config-router)#network 10.10.10.0 mask 255.255.255.252
Router(config-router)#neighbor 10.10.10.1 remote-as 500
Router(config-router)#neighbor 10.10.30.2 remote-as 200
```
`Настройка R5`
```
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#router bgp 500
Router(config-router)#network 192.168.40.0 mask 255.255.255.0
Router(config-router)#network 10.10.10.0 mask 255.255.255.252
Router(config-router)#network 10.10.20.0 mask 255.255.255.252
Router(config-router)#neighbor 10.10.10.2 remote-as 400
Router(config-router)#neighbor 10.10.20.2 remote-as 100
```
### Задание 3
На основе предыдущей лабораторной работы настройте маршрутизацию таким образом, чтобы трафик от R2 проходил через маршрутизаторы: R4->R5->R1.
В качестве ответа приложите примеры конфигураций устройств.

*Приведите ответ в свободной форме.*

# Ответ 

## `route-map` в Cisco Pakcet Tracer отказывается работать.  

1)Вариант настройки атрибута `Weight`

`Настройка Weight R1` 

```
R1(config)#route-map weight-500
R1(config-route-map)#set weight 500
R1(config)#router bgp 100
R1(config-router)#neighbor 10.10.20.1 route-map weight-500 in

```
`Настройка Weight R2` 

```
R1(config)#route-map weight-400
R1(config-route-map)#set weight 400
R1(config)#router bgp 200
R1(config-router)#neighbor 10.10.30.1 route-map weight-400 in

```
1)Вариант настройки атрибута `AS path prepend`

`Настройка AS path prepend на R1` 

```
R1(config)#route-map as-prepend-200
R1(config-route-map)#set as-path prepend 100 100 100
R1(config)#router bgp 100
R1(config-router)#neighbor 10.10.0.2 route-map as-prepend-200 out
```

`Настройка AS path prepend на R2` 

```
R1(config)#route-map as-prepend-100
R1(config-route-map)#set as-path prepend 200 200 200
R1(config)#router bgp 200
R1(config-router)#neighbor 10.10.0.1 route-map as-prepend-100 out
```

### Правила приема домашнего задания

В личном кабинете отправлены:

- ссылка на документ (Google Doc) с выполненным заданием. В документе настроены права доступа “Просматривать могут все в Интернете, у кого есть ссылка”;
- pkt-файл с выполненным проектом.

### Критерии оценки

Зачет - выполнены все задания, ответы даны в развернутой форме, приложены соответствующие скриншоты и файлы проекта, в выполненных заданиях нет противоречий и нарушения логики.

На доработку - задание выполнено частично или не выполнено, в логике выполнения заданий есть противоречия, существенные недостатки.
