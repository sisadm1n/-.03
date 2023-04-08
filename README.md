<h1 align="center"><i>Задание для квалификационного экзамена по ПМ.03. Эксплуатация объектов сетевой инфраструктуры</i></h1>

<i>В условиях импортозамещения организация СAPRICORN имеет гибридную инфраструктуру в офисах двух городов России.</i>

![Image alt](https://github.com/NewErr0r/Qualification_Exam_PM.03/blob/main/images/topologya.png?raw=true)

| Имя устройства | ОС               | IP-адреса                         |
|----------------|-----------------|----------------------------------|
| ISP             | Debian 11       | 4.4.4.1/24, 5.5.5.1/24           |
| RTR-L          | Debian 11       | 4.4.4.100/24, 192.168.200.254/24 |
| RTR-R          | Debian 11       | 5.5.5.100/24, 172.16.100.254/24  |
| SRV-A          | ALTLinux Server 10 | 192.168.200.200/24               |
| CLI-W          | Windows 10 Pro  | DHCP                             |
| DC-W           | Windows Server 2019 | 172.16.100.100/24               |
| CLI-A          | ALTWorkstation 10 | DHCP                             |

## Описание задания:
*Маршрутизатор провайдера настроен и является единой точкой доступа в настоящую сеть Интернет, также предоставляет услуги DNS сервера. Маршрутизатор провайдера является также и сервером времени для контроллера домена в Перми. Контроллер домена в Перми (DC-W) является сервером времени для обоих офисов в Смоленске и Перми.*  

*Все узлы и в Перми, и в Смоленске должны иметь доступ в Интернет.*  

*Поставлена задача объединить офисы в разных городах через VPN. Предпочтительной является связка технологий GRE+IPsec, хотя возможны другие варианты. Об изменении организации VPN следует уведомить заказчика*  

*Между платформами RTR-L и RTR-R должен быть установлен туннель, позволяющий осуществлять связь между регионами с применением внутренних адресов со следующими параметрами:* 
*a) Используйте в качестве VTI интерфейс Tunnel1*  
*b) Между платформами должен быть установлен туннель, позволяющий осуществлять связь между регионами с применением внутренних адресов*  

*Должна использоваться динамическая маршрутизация для связи между офисами.  В Перми и Смоленске.*  

*Внутренний трафик офисов не должен транслироваться в открытые сети.*  

*DC-W должен являться контроллером домена организации и обслуживать DNS-зону второго уровня СAPRICORN.FIRST. Все узлы (в том числе платформы управления трафиком) должны быть доступны по имени.*  

*В каждом офисе должна быть реализована масштабируемая сетевая инфраструктура, поэтому сервер каждого офиса является DHCP-сервером с соответствующим диапазоном.*  

*посредством софтфонов на основе SIP. Номера в Перми должны начинаться с 342, в Смоленске – с 481.  Предпочтительным решением является использование внутренних телефонных станций офисов на основе Asterisk, соединённых между собой. Это обеспечит возможность внутренней телефонной связи внутри офиса при отключении Интернета. Допустимой является реализация телефонной станции Asterisk на одном сервере внутри одного из офисов. О выбранном решении Вы должны уведомить представителя заказчика.*  

*На сервере офиса в Смоленске должен быть развёрнут корпоративный веб-портал, который в целях безопасности запускается в контейнере. Образ (содержащий веб-приложение) и инструкция по работе с приложением расположена в домашней директории текущего пользователя в каталоге «app». Клиенты обоих офисов должны иметь доступ к приложению по имени PORTAL. СAPRICORN.FIRST.*  

*Так как системы под управлением Windows часто подвержены вирусным заражениям, то и на сервере и клиенте под управлением данной операционной системы необходимо организовать мониторинг и антивирусную защиту.*  

*Кроме того, для удобства администратора для управления узлами сети необходимо использовать SNMP и настроить приложение для работы с ним 10-Strike, установленное на контроллере домена в Перми.*  

*При аудите безопасности сторонней организацией оказалось, что в одном из офисов есть подозрительная сетевая активность и утечка данных в Интернет через несанкционированно открытый порт. Необходимо устранить эту уязвимость.*  

*Поступили жалобы от пользователей:*  
*Смоленск, пользователь CLI-W: «Помогите! Горю! Не могу открыть важный файл проекта находящийся в общих документах в папке «проект». Ещё вчера файл открывался, но после того как там поработал другой пользователь файл не открывается. Нужно срочно предоставить его начальнику!»*  

*Пермь, пользователь CLI-A: “Не могу подключится к общей папке на файловом сервере. Там у меня важная информация по текущему проекту. Вроде это на сервере SRV-A В Смоленске. У меня на компьютере это каталог /opt/project должен быть, но он не появляется.»*  


*Примечание. Студент может пользоваться ресурсами сети Интернет из любого клиента офисов в Смоленске или Перми.*  

## Возможные варианты решения:

* Все узлы и в Перми, и в Смоленске должны иметь доступ в Интернет.  
* Внутренний трафик офисов не должен транслироваться в открытые сети.  
  
  
**RTR-L - маршрутизатор в офисе Смоленска**  

*Назначение имени хоста:*  
```
hostnamectl set-hostname RTR-L
```
*Установка часового пояса:*  
```
timedatectl set-timezone Europe/Moscow
```
*Установка пакетов:*  
```
apt install vim firewalld -y
```
*Назначение IP-адреса на внутренний интерфейс:* 
```
vim /etc/network/interfaces
  auto enp0s8
  iface enp0s8 inet static
  address 192.168.200.254
  netmask 255.255.255.0
```
```
systemctl restart networking.service
```
*Включить функцию пересылки пакетов:* 
```
echo net.ipv4.ip_forward=1 >> /etc/sysctl.conf
sysctl -p
```
*Отключение и остановка службы AppArmor:*  
```
systemctl disable --now apparmor.service
```
*enp0s3 - внешний интерфейс (подключение к провайдеру):* 
```
firewall-cmd --permanent --zone=dmz --add-interface=enp0s3
firewall-cmd --permanent --zone=dmz --add-masquerade
```
*enp0s8 - внутренний интерфейс (шлюз - для внутренней сети офиса в Смоленске):*  
```
firewall-cmd --permanent --zone=trusted --add-interface=enp0s8
```
*Применить настройки:*  
```
firewall-cmd --reload
```


**RTR-R - маршрутизатор в офисе Перми**  

*Назначение имени хоста:*  
```
hostnamectl set-hostname RTR-R
```
*Установка часового пояса:*  
```
timedatectl set-timezone Europe/Moscow
```
*Установка пакетов:*  
```
apt install vim firewalld -y
```
*Назначение IP-адреса на внутренний интерфейс:* 
```
vim /etc/network/interfaces
  auto enp0s8
  iface enp0s8 inet static
  address 172.16.100.254
  netmask 255.255.255.0
```
```
systemctl restart networking.service
```
*Включить функцию пересылки пакетов:* 
```
echo net.ipv4.ip_forward=1 >> /etc/sysctl.conf
sysctl -p
```
*Отключение и остановка службы AppArmor:*  
```
systemctl disable --now apparmor.service
```
*enp0s3 - внешний интерфейс (подключение к провайдеру):* 
```
firewall-cmd --permanent --zone=dmz --add-interface=enp0s3
firewall-cmd --permanent --zone=dmz --add-masquerade
```
*enp0s8 - внутренний интерфейс (шлюз - для внутренней сети офиса в Перми):*  
```
firewall-cmd --permanent --zone=trusted --add-interface=enp0s8
```
*Применить настройки:*  
```
firewall-cmd --reload
```


* Поставлена задача объединить офисы в разных городах через VPN. Предпочтительной является связка технологий GRE+IPsec, хотя возможны другие варианты. Об изменении организации VPN следует уведомить заказчика.  
### Вариант через Wireguard:  

**RTR-L**  
*Установка пакета wireguard:*  
```
apt install -y wireguard
```
*Генерируем открытый и закрытый ключи как серверный, так и клиентский:*  
```
mkdir /etc/wireguard/keys
cd /etc/wireguard/keys
wg genkey | tee srv-sec.key | wg pubkey > srv-pub.key
wg genkey | tee cli-sec.key | wg pubkey > cli-pub.key
```
*Перенаправляем содержимое серверного закрытого ключа и клиентского открытого в файл конфигурации для интерфейса WireGuard с именем wg0:*  
```
cat srv-sec.key cli-pub.key >> /etc/wireguard/wg0.conf
```
*Заполняем файл конфигурации для интерфейса WireGuard с именем wg0:*  
```
vim /etc/wireguard/wg0.conf

  [Interface]
  Address = 10.10.10.1/30
  ListenPort = 12345
  PrivateKey = "id_srv-sec.key"

  [Peer]
  PublicKey = "id_cli-pub.key"
  AllowedIPs = 10.10.10.0/30
```
*Включение и запуск интерфейса WireGuard с именем wg0:*  
```
systemctl enable --now wg-quick@wg0
```
*Открыть UDP порт указанный в конфигурационном файле интерфейса wireguard:*  
```
firewall-cmd --permanent --zone=dmz --add-port=12345/udp
firewall-cmd --reload
```
*wg0 интерфейс добавить в другую зону, на нём не должно быть masquerade, иначе все пакеты из одной зоны в другую будут ходить только от имени wg0, такое не допустимо, пусть будет работать динамическая маршрутизация (ospf) + wg0:*    
```
firewall-cmd --permanent --zone=internal --add-interface=wg0
firewall-cmd --reload
```
*Открыть доступ по ssh для передачи клиенту необходимых ключей:*  
```
vim /etc/ssh/sshd_config
  ...
  PermitRootLogin yes
  ...
```
```
systemctl restart sshd
```

**RTR-R**  
*Установка пакета wireguard:*  
```
apt install -y wireguard
```
*Забираем с маршрутизатора офиса в Смоленске открытый ключ сервера и закрытый клиенский ключ:*  
```
mkdir /etc/wireguard/keys
cd /etc/wireguard/keys
scp root@4.4.4.100:/etc/wireguard/cli-sec.key ./
scp root@4.4.4.100:/etc/wireguard/srv-pub.key ./
```
*Перенаправляем содержимое клиентского закрытого ключа и серверного открытого в файл конфигурации для интерфейса WireGuard с именем wg0:*  
```
cat cli-sec.key srv-pub.key >> /etc/wireguard/wg0.conf
```
*Заполняем файл конфигурации для интерфейса WireGuard с именем wg0:*  
```
vim /etc/wireguard/wg0.conf

  [Interface]
  Address = 10.10.10.2/30
  PrivateKey = "id_cli-sec.key"

  [Peer]
  PublicKey = "id_srv-pub.key"
  Endpoint = 4.4.4.100:12345
  AllowedIPs = 10.10.10.0/30
  PersistentKeepalive = 10
```
*Включение и запуск интерфейса WireGuard с именем wg0:*  
```
systemctl enable --now wg-quick@wg0
```
*wg0 интерфейс добавить в другую зону, на нём не должно быть masquerade, иначе все пакеты из одной зоны в другую будут ходить только от имени wg0, такое не допустимо, пусть будет работать динамическая маршрутизация (ospf) + wg0:*    
```
firewall-cmd --permanent --zone=internal --add-interface=wg0
firewall-cmd --reload
```
#### Результат:
![Image alt](https://github.com/NewErr0r/Qualification_Exam_PM.03/blob/main/images/wireguard.png?raw=true)


### Вариант через GRE+IPSec:  
**RTR-L - настройка GRE-туннеля**  
*Добавить GRE для загрузки ядра модулей:*  
```
echo ip_gre >> /etc/modules
```
*Добавить в конец файла конфигурации сетевых интерфейсов блок для GRE:*  
```
vim /etc/network/interfaces
  ...
  auto gre1
  iface gre1 inet tunnel
  address 10.10.10.1
  netmask 255.255.255.252
  mode gre
  local 4.4.4.100
  endpoint 5.5.5.100
  ttl 65
```
```
systemctl restart networking.service
```
*GRE интерфейс добавить в другую зону, на нём не должно быть masquerade, иначе все пакеты из одной зоны в другую будут ходить только от имени GRE, такое не допустимо, пусть будет работать динамическая маршрутизация (ospf) + GRE:*    
```
firewall-cmd --permanent --zone=internal --add-interface=gre1
firewall-cmd --permanent --zone=interfal --add-protocol=gre
firewall-cmd --permanent --zone=dmz --add-protocol=gre
firewall-cmd --reload
```
**RTR-L - настройка IPSec**
*Установка пакета:*  
```
apt install -y strongswan
systemctl enable --now strongswan
```
*Правим конфигурационный файл IPsec*  
```
vim /etc/ipsec.conf
  
  config setup
      strictcrlpolicy=no
      uniqueids = yes
      
  conn gre
      type=tunnel
      authby=secret
      left=10.10.10.1
      right=10.10.10.2
      leftprotoport=gre
      rightprotoport=gre
      ike=aes256-sha2_256-modp1024!
      esp=aes256-sha2_256!
      auto=start
      pfs=no
```
*Настраиваем общий секрет:*  
```
vim /etc/ipsec.secrets
  ...
  10.10.10.1 10.10.10.2 : PSK "P@ssw0rd"
  ...
```
```
systemctl restart strongswan
```
*Настройка firewalld для работы с IPSec:*  
```
firewall-cmd --permanent --zone=dmz --add-service=ipsec
firewall-cmd --permanent --zone=dmz --add-protocol=esp
firewall-cmd --permanent --zone=internal --add-service=ipsec
firewall-cmd --permanent --zone=internal --add-protocol=esp
firewall-cmd --reload
```


**RTR-R - настройка GRE-туннеля**  
*Добавить GRE для загрузки ядра модулей:*  
```
echo ip_gre >> /etc/modules
```
*Добавить в конец файла конфигурации сетевых интерфейсов блок для GRE:*  
```
vim /etc/network/interfaces
  ...
  auto gre1
  iface gre1 inet tunnel
  address 10.10.10.2
  netmask 255.255.255.252
  mode gre
  local 5.5.5.100
  endpoint 4.4.4.100
  ttl 65
```
```
systemctl restart networking.service
```
*GRE интерфейс добавить в другую зону, на нём не должно быть masquerade, иначе все пакеты из одной зоны в другую будут ходить только от имени GRE, такое не допустимо, пусть будет работать динамическая маршрутизация (ospf) + GRE:*    
```
firewall-cmd --permanent --zone=internal --add-interface=gre1
firewall-cmd --permanent --zone=interfal --add-protocol=gre
firewall-cmd --permanent --zone=dmz --add-protocol=gre
firewall-cmd --reload
```
**RTR-R - настройка IPSec**
*Установка пакета:*  
```
apt install -y strongswan
systemctl enable --now strongswan
```
*Правим конфигурационный файл IPsec*  
```
vim /etc/ipsec.conf
  
  config setup
      strictcrlpolicy=no
      uniqueids = yes
      
  conn gre
      type=tunnel
      authby=secret
      left=10.10.10.2
      right=10.10.10.1
      leftprotoport=gre
      rightprotoport=gre
      ike=aes256-sha2_256-modp1024!
      esp=aes256-sha2_256!
      auto=start
      pfs=no
```
*Настраиваем общий секрет:*  
```
vim /etc/ipsec.secrets
  ...
  10.10.10.2 10.10.10.1 : PSK "P@ssw0rd"
  ...
```
```
systemctl restart strongswan
```
*Настройка firewalld для работы с IPSec:*  
```
firewall-cmd --permanent --zone=dmz --add-service=ipsec
firewall-cmd --permanent --zone=dmz --add-protocol=esp
firewall-cmd --permanent --zone=internal --add-service=ipsec
firewall-cmd --permanent --zone=internal --add-protocol=esp
firewall-cmd --reload
```
#### Результат:
![Image alt](https://github.com/NewErr0r/Qualification_Exam_PM.03/blob/main/images/GRE_IPSec.png?raw=true)


* Должна использоваться динамическая маршрутизация для связи между офисами.  В Перми и Смоленске.  

**RTR-L**  
*Установка пакета:*  
```
apt install frr -y
```
*Включение демона ospfd:*  
```
vim /etc/frr/daemons
  ...
  ospfd=yes
  ...
```
```
systemctl restart frr
```
*Настройка OSPF:*  
```
vtysh
conf t
ip route 0.0.0.0 0.0.0.0 4.4.4.1
interface <gre1|wg0>
access-list 1 permit 192.168.200.0 0.0.0.255
router ospf
passive-nterface default
no passive-interface <gre1|wg0>
network 10.10.10.0/30 area 0
network 192.168.200.0/24 area 0
exit
do wr mem
```
*Назначаем разрешения в firewalld для работы OSPF:*  
```
firewall-cmd --permanent --zone=dmz --add-protocol=ospf
firewall-cmd --permanent --zone=internal --add-protocol=ospf
firewall-cmd --reload
```

**RTR-R**    
*Установка пакета:*  
```
apt install frr -y
```
*Включение демона ospfd:*  
```
vim /etc/frr/daemons
  ...
  ospfd=yes
  ...
```
```
systemctl restart frr
```
*Настройка OSPF:*  
```
vtysh
conf t
ip route 0.0.0.0 0.0.0.0 5.5.5.1
interface <gre1|wg0>
access-list 1 permit 172.16.100.0 0.0.0.255
router ospf
passive-nterface default
no passive-interface <gre1|wg0>
network 10.10.10.0/30 area 0
network 172.16.100.0/24 area 0
exit
do wr mem
```
*Назначаем разрешения в firewalld для работы OSPF:*  
```
firewall-cmd --permanent --zone=dmz --add-protocol=ospf
firewall-cmd --permanent --zone=internal --add-protocol=ospf
firewall-cmd --reload
```
#### Результат:
![Image alt](https://github.com/NewErr0r/Qualification_Exam_PM.03/blob/main/images/OSPF.png?raw=true)


* Контроллер домена в Перми (DC-W) является сервером времени для обоих офисов в Смоленске и Перми.  

**DC-W**  
*Проверить настройки сетевого Адаптера:*  

![Image alt](https://github.com/NewErr0r/Qualification_Exam_PM.03/blob/main/images/DC-W_IP.png?raw=true)

*Настроить синхранизацию времени с провайдером - ISP:*  

![Image alt](https://github.com/NewErr0r/Qualification_Exam_PM.03/blob/main/images/DC-W_NTP.png?raw=true)

**RTR-L и RTR-R**  
*Настройка синзронизации времени с DC-W:*  
```
apt install chrony
```
```
vim /etc/chrony/chrony.conf
  ...
  server 172.16.100.100 iburst prefer
  allow 192.168.200.0/24
  allow 172.16.100.0/24
```
```
systemctl restart chronyd
```
*Настройка firewalld на работу NTP (chrony):*  
```
firewall-cmd --permanent --zone=dmz --add-port=123/udp
firewall-cmd --permanent --zone=trusted --add-service=ntp
firewall-cmd --permannet --zone=internal --add-port=123/udp
firewall-cmd --reload
```


* DC-W должен являться контроллером домена организации и обслуживать DNS-зону второго уровня СAPRICORN.FIRST.
*Установка необходимых ролей:*  

![Image alt](https://github.com/NewErr0r/Qualification_Exam_PM.03/blob/main/images/DC-W_roles.png?raw=true)

*Повышение до уровня контроллера домена:*  

![Image alt](https://github.com/NewErr0r/Qualification_Exam_PM.03/blob/main/images/DC-W_ADDC1.png?raw=true)

![Image alt](https://github.com/NewErr0r/Qualification_Exam_PM.03/blob/main/images/DC-W_ADDC2.png?raw=true)

![Image alt](https://github.com/NewErr0r/Qualification_Exam_PM.03/blob/main/images/DC-W_ADDC3.png?raw=true)

*Управление доменом:*   

![Image alt](https://github.com/NewErr0r/Qualification_Exam_PM.03/blob/main/images/DC-W_ADAdministration.png?raw=true)

*Настройка DNS-сервера:*   

*Создание зон обратного просмотра:*  

![Image alt](https://github.com/NewErr0r/Qualification_Exam_PM.03/blob/main/images/reverce_zone200.png?raw=true)

![Image alt](https://github.com/NewErr0r/Qualification_Exam_PM.03/blob/main/images/reverce_zone100.png?raw=true)

*Добавления необходимых записей в зону прямого просмотра:*  

![Image alt](https://github.com/NewErr0r/Qualification_Exam_PM.03/blob/main/images/DC-W_DNS.png)

* Все узлы (в том числе платформы управления трафиком) должны быть доступны по имени.

**RTR-L и RTR-R**
```
vim /etc/resolv.conf
  search CAPRICORN.FIRST
  nameserver 172.16.100.100
```

#### Результат:
![Image alt](https://github.com/NewErr0r/Qualification_Exam_PM.03/blob/main/images/DNS.png?raw=true)




* В каждом офисе должна быть реализована масштабируемая сетевая инфраструктура, поэтому сервер каждого офиса является DHCP-сервером с соответствующим диапазоном.

**DC-W**  
*Настройка DHCP сервера для офиса в Перми:*  

![Image alt](https://github.com/NewErr0r/Qualification_Exam_PM.03/blob/main/images/DC-W_DHCP1.png?raw=true)


![Image alt](https://github.com/NewErr0r/Qualification_Exam_PM.03/blob/main/images/DC-W_DHCP2.png?raw=true)


![Image alt](https://github.com/NewErr0r/Qualification_Exam_PM.03/blob/main/images/DC-W_DHCP3.png?raw=true)


![Image alt](https://github.com/NewErr0r/Qualification_Exam_PM.03/blob/main/images/DC-W_DHCP4.png?raw=true)

#### Результат с CLI-A:

![Image alt](https://github.com/NewErr0r/Qualification_Exam_PM.03/blob/main/images/CLI-A_DHCP.png?raw=true)

**SRV-A**  
*Настройка DHCP сервера для офиса в Смоленске:*  

*Проверка параметров сетевого Адаптера:*

![Image alt](https://github.com/NewErr0r/Qualification_Exam_PM.03/blob/main/images/SRV-A_IP.png?raw=true)

*Установка пакета:*  
```
apt-get update && apt-get install -y dhcp-server
```

*Настройка конфигурационного файла:*  
```
cp /etc/dhcp/dhcpd.conf.sample /etc/dhcp/dhcpd.conf
vim /etc/dhcp/dhcpd.conf
  
  ddns-update-style none;

  subnet 192.168.200.0 netmask 255.255.255.0 {
        option routers                  192.168.200.254;
        option subnet-mask              255.255.255.0;

        option nis-domain               "CAPRICORN.FIRST";
        option domain-name              "CAPRICORN.FIRST";
        option domain-name-servers      172.16.100.100;

        range dynamic-bootp 192.168.200.50 192.168.200.80;
        default-lease-time 21600;
        max-lease-time 43200;
}
```
```
systemctl enable --now dhcpd.service
```
#### Результат с CLI-W:  

![Image alt](https://github.com/NewErr0r/Qualification_Exam_PM.03/blob/main/images/CLI-W_DHCP.png?raw=true)

## Ввод в домен:  

*Ввод в домен CLI-A:* 

![Image alt](https://github.com/NewErr0r/Qualification_Exam_PM.03/blob/main/images/CLI-A-AD.png?raw=true)

*Ввод в домен CLI-W:* 

![Image alt](https://github.com/NewErr0r/Qualification_Exam_PM.03/blob/main/images/CLI-W_AD.png?raw=true)

*Ввод в домен SRV-A:* 
```
apt-get install -y task-auth-ad-sssd
```
![Image alt](https://github.com/NewErr0r/Qualification_Exam_PM.03/blob/main/images/SRV-A_AD.png?raw=true)

#### Результат:  

![Image alt](https://github.com/NewErr0r/Qualification_Exam_PM.03/blob/main/images/DC-W_ADComputers.png?raw=true)


* Между клиентскими компьютерами офисов должна быть реализована телефонная связь посредством софтфонов на основе SIP. Номера в Перми должны начинаться с 342, в Смоленске – с 481.  
* Предпочтительным решением является использование внутренних телефонных станций офисов на основе Asterisk, соединённых между собой.  

**RTR-L**
```
#
```

**RTR-R**
```
#
```
* На сервере офиса в Смоленске должен быть развёрнут корпоративный веб-портал, который в целях безопасности запускается в контейнере.  

**SRV-A**
```
#
```

* Клиенты обоих офисов должны иметь доступ к приложению по имени PORTAL. СAPRICORN.FIRST.

**DC-W**
```
# 
```

* Кроме того, для удобства администратора для управления узлами сети необходимо использовать SNMP и настроить приложение для работы с ним 10-Strike, установленное на контроллере домена в Перми.  

**DC-W**
```
#
```

