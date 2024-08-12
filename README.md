# Домашнее задание к занятию "Построение виртуальных частных сетей. Протоколы обеспечения защиты передаваемых данных"

### Цель задания

Данное домашнее задание позволит научиться строить ВПН каналы между маршрутизаторами и устанавливать связность между локальными сетями. А также углубиться в технологию инкапсуляции пакетов.

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

### Лабораторная работа "Конфигурация IPSeс туннулей и инкапсляции трафика"

Можете использовать симулятор для построения топологии и поиска/проверки ответов на задания. 

------

### Задание 1. 

На картинке изображена схема сети, состоящих из трех офисов.
![image](https://user-images.githubusercontent.com/51816695/165988267-fa9bdd4a-a848-45ea-9567-78187b49343c.png)
Необходимо создать туннели между каждым офисом.
1) Между R1-R2 создать ВПН канал и обеспечить связность между клиентами маршрутизаторов по зашифрованному каналу со следующими настройками:

ISAKMP(Ikev1) Phase 1 протоколы:
 - шифрование 3des
 - хеширование md5
 - аутентификация pre-share
 - Диффи-Хелманн группа: 2
 - время жизни 3600 секунд

IPSEC Transform-set:
 - ESP: esp-3des
 - AH: ah-md5-hmac
 - Mode: Tunnel
 
2) Между R1-R3 создать ВПН канал и обеспечить связность между клиентами маршрутизаторов по зашифрованному каналу со следующими настройками:

ISAKMP(Ikev1) Phase 1 протоколы:
- шифрование aes 256
- хеширование sha512
- аутентификация pre-share
- Диффи-Хелманн группа: 5
- время жизни 3600 секунд
 
 IPSEC Transform-set:
 - ESP: esp-aes 256
 - ESP-AH: esp-sha512-hmac
 - AH: ah-sha256-hmac
 - Mode: Tunnel

 3) Между R2-R3 создать ВПН канал и обеспечить связность между клиентами маршрутизаторов по зашифрованному каналу со следующими настройками:

ISAKMP(Ikev1) Phase 1 протоколы:
 - шифрование aes 256
 - хеширование sha512
 - аутентификация pre-share
 - Диффи-Хелманн группа: 5
 - время жизни 3600 секунд
 
 IPSEC Transform-set:
- ESP:  esp-aes 256
- ESP-AH: esp-sha256-hmac
- AH: ah-sha-hmac
- Mode: Tunnel


*Отправьте полный список конфигураций и политик. Требуется, чтоб все три туннеля могли работать одновременно.*

------
# Ответ 

# Общая топология сети 

# ![images1]()


# [Скачать Файл.pkt]()

# настройка динамической маршрутизации `OSPF`

`R1`

```
R1#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
R1(config)#router ospf 1
R1(config)#interface gigabitEthernet 0/0/0
R1(config-if)#ip ospf 1 area 0
R1(config)#interface gigabitEthernet 0/0/1
R1(config-if)#ip ospf 1 area 0
R1(config-if)#
```
`R2`

```
R2#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
R2(config)#router ospf 1
R2(config)#interface gigabitEthernet 0/0/0
R2(config-if)#ip ospf 1 area 0
R2(config)#interface gigabitEthernet 0/0/1
R2(config-if)#ip ospf 1 area 0
R2(config-if)#
```
`R3`

```
R3#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
R3(config)#router ospf 1
R3(config)#interface gigabitEthernet 0/0/0
R3(config-if)#ip ospf 1 area 0
R3(config)#interface gigabitEthernet 0/0/1
R3(config-if)#ip ospf 1 area 0
R3(config-if)#
```
# Cоздание туннелей между каждым офисом
# R1-R2

`R1`
```
R1(config)#access-list 110 permit ip 192.168.5.0 0.0.0.255 172.16.1.0 0.0.0.255
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
R1(config)#interface gigabitEthernet 0/0/0
R2(config-if)#crypto map VPN-MAP
```
`R2`

```
R2(config)#access-list 110 permit ip 172.16.1.0 0.0.0.255 192.168.0.0 0.0.0.255
R2(config)#crypto isakmp policy 10
R2(config-isakmp)#encryption 3des 
R2(config-isakmp)#authentication pre-share 
R2(config-isakmp)#hash md5
R2(config-isakmp)#group 2
R2(config-isakmp)#Lifetime 3600
R2(config-isakmp)#ex
R2(config)#crypto isakmp key cisco address 100.0.0.2
R2(config)#crypto ipsec  transform-set VPN-SET ah-md5-hmac esp-3des
R2(config)#crypto map VPN-MAP 10 ipsec-isakmp 
% NOTE: This new crypto map will remain disabled until a peer
        and a valid access list have been configured.
R2(config-crypto-map)#description VPN connection to R2
R2(config-crypto-map)#set peer 100.0.0.1
R2(config-crypto-map)#set transform-set VPN-SET
R2(config-crypto-map)#match address 110
R2(config-crypto-map)#ex
R2(config)#interface gigabitEthernet 0/0/0
R2(config-if)#crypto map VPN-MAP
```
*До отправки пакета по маршруту R1-R2*
# ![images2]()
*После отправки пакета по маршруту R1-R2*
# ![images3]()


## R2-R3

`R2`

```
R2(config)#crypto isakmp policy 20
R2(config-isakmp)#encryption aes 256 
R2(config-isakmp)#authentication pre-share 
R2(config-isakmp)#hash sha
R2(config-isakmp)#group 5
R2(config-isakmp)#Lifetime 3600
R2(config-isakmp)#ex
R2(config)#ip access-list extended VPN-R3
R2(config-ext-nacl)#permit ip 172.16.1.0 0.0.0.255 10.10.10.0 0.0.0.255
R2(config-ext-nacl)#ex
R2(config)#crypto isakmp key cisco2 address 100.0.0.3
R2(config)#crypto ipsec transform-set VPN-SET2 esp-aes esp-sha-hmac
R2(config)#crypto map VPN-MAP 20 ipsec-isakmp 
% NOTE: This new crypto map will remain disabled until a peer
        and a valid access list have been configured.
R2(config-crypto-map)#description VPN connection to R2
R2(config-crypto-map)#set peer 100.0.0.3
R2(config-crypto-map)#set transform-set VPN-SET2
R2(config-crypto-map)#match address 110
R2(config-crypto-map)#ex
```
`R3` 

```
R3(config)#crypto isakmp policy 10
R3(config-isakmp)#encryption aes 256 
R3(config-isakmp)#authentication pre-share 
R3(config-isakmp)#hash sha
R3(config-isakmp)#group 5
R3(config-isakmp)#Lifetime 3600
R3(config-isakmp)#ex
R3(config)#ip access-list extended VPN-R3
R3(config-ext-nacl)#permit ip 10.10.10.0 0.0.0.255 172.16.1.0 0.0.0.255
R3(config-ext-nacl)#ex
R3(config)#crypto isakmp key cisco2 address 100.0.0.2
R3(config)#crypto ipsec transform-set VPN-SET2 esp-aes esp-sha-hmac
R3(config)#crypto map VPN-MAP 20 ipsec-isakmp 
% NOTE: This new crypto map will remain disabled until a peer
        and a valid access list have been configured.
R3(config-crypto-map)#description VPN connection to R2
R3(config-crypto-map)#set peer 100.0.0.2
R3(config-crypto-map)#set transform-set VPN-SET2
R3(config-crypto-map)#match address VPN-R3
R3(config-crypto-map)#ex
R3(config)#interface gigabitEthernet 0/0/0
R3(config-if)#crypto map VPN-MAP

```
*До отправки пакета по маршруту R2-R3*
# ![images4]()
*После отправки пакета по маршруту R2-R3*
# ![images5]()

## R3-R1

`R3`

```
R3(config)#crypto isakmp policy 20
R3(config-isakmp)#encryption aes 256 
R3(config-isakmp)#authentication pre-share 
R3(config-isakmp)#hash sha
R3(config-isakmp)#group 5
R3(config-isakmp)#Lifetime 3600
R3(config-isakmp)#ex
R3(config)#ip access-list extended VPN-R1
R3(config-ext-nacl)#permit ip 10.10.10.0 0.0.0.255 192.168.5.0 0.0.0.255
R3(config-ext-nacl)#ex
R3(config)#crypto isakmp key cisco3 address 100.0.0.1
R3(config)#crypto ipsec transform-set VPN-SET1 esp-aes esp-sha-hmac
R3(config)#crypto map VPN-MAP 20 ipsec-isakmp 
% NOTE: This new crypto map will remain disabled until a peer
        and a valid access list have been configured.
R3(config-crypto-map)#description VPN connection to R1
R3(config-crypto-map)#set peer 100.0.0.1
R3(config-crypto-map)#set transform-set VPN-SET1
R3(config-crypto-map)#match address VPN-R1
R3(config-crypto-map)#ex
```
`R1`
 
```
R1(config)#crypto isakmp policy 20
R1(config-isakmp)#encryption aes 256 
R1(config-isakmp)#authentication pre-share 
R1(config-isakmp)#hash sha
R1(config-isakmp)#group 5
R1(config-isakmp)#Lifetime 3600
R1(config-isakmp)#ex
R1(config)#ip access-list extended VPN-R3
R1(config-ext-nacl)#permit ip 192.168.5.0 0.0.0.255 10.10.10.0 0.0.0.255
R1(config-ext-nacl)#ex
R1(config)#crypto isakmp key cisco3 address 100.0.0.3
R1(config)#crypto ipsec transform-set VPN-SET3 esp-aes esp-sha-hmac
R1(config)#crypto map VPN-MAP 20 ipsec-isakmp 
% NOTE: This new crypto map will remain disabled until a peer
        and a valid access list have been configured.
R1(config-crypto-map)#description VPN connection to R3
R1(config-crypto-map)#set peer 100.0.0.3
R1(config-crypto-map)#set transform-set VPN-SET3
R1(config-crypto-map)#match address VPN-R3
R1(config-crypto-map)#ex
```
*До отправки пакета по маршруту R3-R1*
# ![images6]()
*После отправки пакета по маршруту R3-R1*
# ![images7]()



### Задание 2. 

По топологии из задания 1 построить туннели инкапсуляции между маршрутизаторами.

1) Пакеты в инкапсуляции должны использовать адреса Loopback1 интерфейсов в качестве source и destination.
2) Трафик между клиентами надо инкапсулировать в данные туннели.

*Отправьте список маршрутизаторов со списком команд, необходимых для выполнения.*

# Ответ 
# [Скачать Файл.pkt]()

# Создание loopback 0 

`R1`

```
R1#configure terminal 
R1(config)#interface loopback 0
R1(config-if)#ip address 1.1.1.1 255.255.255.255
```

`R2`

```
R2#configure terminal 
R2(config)#interface loopback 0
R2(config-if)#ip address 2.2.2.2 255.255.255.255
```
`R3`

```
R3#configure terminal 
R3(config)#interface loopback 0
R3(config-if)#ip address 3.3.3.3 255.255.255.255
```

# Настройка тунеля с маршрутизацией.

## R1-R2

`R1`

```
R1#configure terminal 
R1(config)#interface tunnel 0
R1(config-if)#ip address 10.1.1.1 255.255.255.0
R1(config-if)#tunnel source loopback 0
R1(config-if)#tunnel destination 2.2.2.2
R1(config-if)#ex
R1(config)#
R1(config)#ip route 2.2.2.2 255.255.255.255 100.0.0.2
R1(config)# ip route 172.16.1.0 255.255.255.0 2.2.2.2
R1(config)#
```
`R2`

```
R2#configure terminal 
R2(config)#interface tunnel 0
R2(config-if)#ip address 10.1.1.2 255.255.255.0
R2(config-if)#tunnel source loopback 0
R2(config-if)#tunnel destination 1.1.1.1
R2(config-if)#ex
R2(config)#
R2(config)#ip route 1.1.1.1 255.255.255.255 100.0.0.1
R2(config)# ip route 192.168.5.0 255.255.255.0 1.1.1.1
R2(config)#
```


## R2-R3

`R2`

```
R2#configure terminal 
R2(config)#interface tunnel 1
R2(config-if)#ip address 10.1.2.1 255.255.255.0
R2(config-if)#tunnel source loopback 0
R2(config-if)#tunnel destination 3.3.3.3
R2(config-if)#ex
R2(config)#
R2(config)#ip route 3.3.3.3 255.255.255.255 100.0.0.3
R2(config)# ip route 10.10.10.0 255.255.255.0 3.3.3.3
R2(config)#
```
`R3`

```
R3#configure terminal 
R3(config)#interface tunnel 1
R3(config-if)#ip address 10.1.2.2 255.255.255.0
R3(config-if)#tunnel source loopback 0
R3(config-if)#tunnel destination 2.2.2.2
R3(config-if)#ex
R3(config)#
R3(config)#ip route 2.2.2.2 255.255.255.255 100.0.0.2
R3(config)# ip route 172.16.1.0 255.255.255.0 2.2.2.2
R3(config)#
```
## R3-R1

`R3`

```
R3#configure terminal 
R3(config)#interface tunnel 2
R3(config-if)#ip address 10.1.3.1 255.255.255.0
R3(config-if)#tunnel source loopback 0
R3(config-if)#tunnel destination 1.1.1.1
R3(config-if)#ex
R3(config)#
R3(config)#ip route 1.1.1.1 255.255.255.255 100.0.0.1
R3(config)# ip route 192.168.5.0 255.255.255.0 1.1.1.1
R3(config)#
```
`R1`

```
R1#configure terminal 
R1(config)#interface tunnel 2
R1(config-if)#ip address 10.1.3.2 255.255.255.0
R1(config-if)#tunnel source loopback 0
R1(config-if)#tunnel destination 3.3.3.3
R1(config-if)#ex
R1(config)#
R1(config)#ip route 3.3.3.3 255.255.255.255 100.0.0.3
R1(config)# ip route 10.10.10.0 255.255.255.0 3.3.3.3
R1(config)#
```



### Правила приема домашнего задания

В личном кабинете отправлена ссылка на документ (Google Doc) с выполненным заданием. В документе настроены права доступа “Просматривать могут все в Интернете, у кого есть ссылка”.

### Критерии оценки

Зачет - выполнены все задания, ответы даны в развернутой форме, приложены соответствующие скриншоты и файлы проекта, в выполненных заданиях нет противоречий и нарушения логики.

На доработку - задание выполнено частично или не выполнено, в логике выполнения заданий есть противоречия, существенные недостатки.
