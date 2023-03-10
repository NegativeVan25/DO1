## Сети Linux/LinuxNetwork.

## Part 1. Инструмент **ipcalc**


-  **Машина создана с нужным именем**

![Имя машины](img1/0.png)

- ### **1.1: Сети и маски**
- **установка ipcalc**

<br> 

![Созданная машина](img1/1.png)

- ### **1.1.1 Адрес сети:** *192.167.38.54/13*
<br>*Результат работы команды ipcalc по этим данным*<br>

![Результат работы 192.167.38.54/13](img1/2.png)

- ### **1.1.2 Перевод маски 255.255.255.0 в префиксную и двоичную запись, /15 в обычную и двоичную, 11111111.11111111.11111111.11110000 в обычную и префиксную:**

<br> Перевод маски: 255.255.255.0

*Отображение в консоли*

![Перевод маски 255.255.255.0](img1/3.png)

 - **Префиксная запись:** 24
 - **Двоичная запись:** 11111111.11111111.11111111.00000000

<br>    Перевод прeфиксной записи: /15

![Перевод прeфиксной записи /15](img1/4.png)

- **Обычная запись:** 255.254.0.0
- **Двоичная запись:** 11111111.11111110.00000000.00000000

<br>    Перевод бинарной записи; 11111111.11111111.11111111.11110000:

**Здесь уже придется считать вручную и без скринов**

- **Прeфикс**: /28 (Количество активных едениц подряд)
- **Маска**: 255.255.255.240 (Что соответствует прификсу /28)
<br> 

- ### **1.1.3: Минимальный и максимальный хост в сети *12.167.38.4* при масках:** 
<br>
- */8*:

![Min, max host /8](img1/5.png)

- **HostMin:** 12.0.0.1 
- **HostMax:** 12.255.255.254
<br>

- */11111111.11111111.00000000.00000000*:

![Min, max host /11111111.11111111.00000000.00000000](img1/6.png)

Если переводить в десятичную запись, то маска выглядит следующим образом: 255.255.0.0

- **HostMin:** 12.167.0.1 
- **HostMax:** 12.167.255.254
<br>

- */255.255.254.0*:

![Min, max host /255.255.254.0](img1/7.png)

- **HostMin:** 12.167.38.1 
- **HostMax:** 12.167.39.254
<br>

- */4*:

![Min, max host /8](img1/8.png)

- **HostMin:** 0.0.0.1 
- **HostMax:** 15.255.255.254
<br>

- ### **1.2: Localhost. Определить и записать в отчёт, можно ли обратиться к приложению, работающему на localhost, со следующими IP: 194.34.23.100, 127.0.0.2, 127.1.0.1, 128.0.0.1:** 
<br>

- **Минимальный доступный адрес locahost:** - 127.0.0.1

![ipcalc 127.0.0.1](img1/9.png)

 **Адрес имеет класс A, следовательно его маска по умолчанию :** /8

![ipcalc 127.0.0.1/8](img1/10.png)

 **Нам доступен диапозон :** 
 **Min:** 127.0.0.1 
 **Max:** 127.255.255.254

 **Под наши цели подойдут следующие ip-адреса :** 127.0.0.2 , 127.1.0.1
<br>

 - ### **1.3. Диапазоны и сегменты сетей:** 
<br>

- ### **1.3.1 Какие из перечисленных IP можно использовать в качестве публичного, а какие только в качестве частных: 10.0.0.45, 134.43.0.2, 192.168.4.2, 172.20.250.4, 172.0.2.1, 192.172.0.1, 172.68.0.2, 172.16.255.255, 10.10.10.10, 192.169.168.1:** 
<br>

 **Только частные адреса:** 10.0.0.45 , 192.168.4.2 , 172.20.250.4 , 172.16.255.255 , 10.10.10.10
 **Адреса, которые могут быть публичными:** 134.43.0.2 , 172.0.2.1 , 192.172.0.1 , 172.68.0.2 , 192.169.168.1

<br>

- ### **1.3.2 Какие из перечисленных IP адресов шлюза возможны у сети 10.10.0.0/18: 10.0.0.1, 10.10.0.2, 10.10.10.10, 10.10.100.1, 10.10.1.255:** 
<br>

- **Узнаем min и max для этой сети:**

![ipcalc 10.10.0.0/18](img1/11.png)

**Нам доступен диапозон :** 
 **Min:** 10.10.0.1 
 **Max:** 10.10.63.254

**Под наши цели подойдут следующие ip-адреса :** 10.10.100.1 и 10.0.0.1


## Part 2. Статическая маршрутизация между двумя машинами

- **Поднять две виртуальные машины (далее -- ws1 и ws2) и с помощью команды ip a посмотреть существующие сетевые интерфейсы** 
<br>

![2 по цене 1](img2/2.png)

- **Описать сетевой интерфейс, соответствующий внутренней сети, на обеих машинах и задать следующие адреса и маски: ws1 - `192.168.100.10, маска /16`, ws2 - `172.24.116.8, маска /12`** 
<br>

- **Редактируем соответствующие файлы в netplan на каждой машине**

<br>

![Редактируем netplans](img2/3.1.png)


- **Выполнить команду `netplan apply` для перезапуска сервиса сети**

<br>

![Применяем изменения](img2/3.3.png)

- ### **2.1. Добавление статического маршрута вручную**
<br>

- **Добавить статический маршрут от одной машины до другой и обратно при помощи команды вида `ip r add`**
<br>

- **Используем команду `sudo ip r add [нужный ip] dev enp0s3` на каждой машние и пингуем их**
Для первой машины используем ip второй и наоборот.


![Статический коннект 2х машин](img2/4.png)

- ### **2.2. Добавление статического маршрута с сохранением.**

- **Ребутаем машины**
<br>

![Ребутаем машины](img2/5.png)

<br>

- **Меняем файл etc/netplan/00-installer-config.yaml на обеих машинах**
<br>

![Еще 1 изменение netplan](img2/6.png)

- **Пингуем**
<br>

![Проверяем успешность коннекта 2х машин](img2/7.png)
<br>


## Part 3. Утилита **iperf3**

- ### **3.1. Скорость соединения**
<br>
Перевести и записать в отчёт: 8 Mbps в MB/s, 100 MB/s в Kbps, 1 Gbps в Mbps
<br>

`8 Mbps` = `1 MB/s`
`100 MB/s` = `819200 Kbps`
`1 Gbps` = `1024 Mbps`

- ### **3.2. Утилита iperf3**
<br>

- **Для того, чтобы проверить скорость соединения c помощью `iperf3` нужно сделать одну машину сервером(ws1), а другую клиентом (ws2)**
<br>

- **Устанавливаем на обе машины ipef3 с помощью команды `sudo apt install iperf3`**
<br>

- **Используем команду `iperf3 -s` на машине которая будет сервером**
<br>

- **После определения машины-сервера используем команду `iperf3 -c [ip машины -сервера]` на клиентской машине**
<br>

- *Результаты на скриншоте*

![Проверяем успешность коннекта 2х машин](img3/1.png)

## Part 4. Сетевой экран
<br>

- ### **4.1. Утилита iptables**
<br>

Создать файл `/etc/firewall.sh`, имитирующий фаерволл, на ws1 и ws2:

` #!/bin/sh`

` # Удаление всех правил в таблице "filter" (по-умолчанию). `
` iptables –F `
` iptables -X `

- **Нужно добавить в файл подряд следующие правила:**

1) на ws1 применить стратегию когда в начале пишется запрещающее правило, а в конце пишется разрешающее правило (это касается пунктов 4 и 5)

2) на ws2 применить стратегию когда в начале пишется разрешающее правило, а в конце пишется запрещающее правило (это касается пунктов 4 и 5)

3) открыть на машинах доступ для порта 22 (ssh) и порта 80 (http)

4) запретить echo reply (машина не должна "пинговаться”, т.е. должна быть блокировка на OUTPUT)

5) разрешить echo reply (машина должна "пинговаться")

![Firewall](img4/1.png)

- **Запустить файлы на обеих машинах командами `chmod +x /etc/firewall.sh` и `/etc/firewall.sh`**

![Загружаем Firewall](img4/2.png)

- ### **4.2. Утилита `nmap`**
<br>

- **Устанавливаем утилиту `nmap`**
<br>

- **Командой ping найти машину, которая не "пингуется", после чего утилитой nmap показать, что хост машины запущен**
<br>

![Пингуем машины](img4/3.png)

- Не пингуется машина с ip 192.168.100.10 (ws1)
<br>

![nmap нерабочей машины](img4/4.png)

- Порты для подключения к 192.168.100.10 закрыты.
<br>

## Part 5. Статическая маршрутизация сети

- Поднять пять виртуальных машин (3 рабочие станции (ws11, ws21, ws22) и 2 роутера (r1, r2))

<br>

- ### **5.1. Настройка адресов машин**

<br>

- Настроить конфигурации машин в etc/netplan/00-installer-config.yaml согласно сети на рисунке в задании.
- Создав все машины, мы редактируем netplan через *nano*.
- Далее применяем команду *sudo netplan apply* что бы изменения вступили в силу.
- Выключаем машины и внастройках виртуал бокс добавляем по одному одаптеру к роутерам. Это неоюходимо для коректной работы доплонительных сетевых карт.
- Проверяем все сделаное.

<br>

- **Результат настройки машин**
- **ws11.**
<br>

![ws11](img5/ws11.png)
- **r1.**
<br>

![r1](img5/r1.png)
- **ws21.**
<br>

![ws21](img5/ws21.png)
- **ws22.**
<br>

![ws22](img5/ws22.png)
- **r2.**
<br>

![r2](img5/r2.png)

<br>

- **Пингуем ws11 с r1**
![ws11r1](img5/5.1pws11.png)
<br>

- **Пингуем ws21 с ws22**
![ws21ws22](img5/5.1pws21.png)
![ws22ws21](img5/5.1pws22.png)
- ### **5.2. Включение переадресации IP-адресов.**

- **Применяем команду `sysctl -w net.ipv4.ip_forward=1` на роутерах**
<br>

- **Результат работы на r1.**

![Результат на r1](img5/1.png)

- **Результат работы на r2.**

![Результат на r2](img5/2.png)

- **Открывем файл `/etc/sysctl.conf ` и добавляем в него строку ` net.ipv4.ip_forward = 1`**

- **Результат редактирования на r1.**

![Редактирование на r1](img5/3.png)

- **Результат редактирования на r2.**

![Редактирование на r2](img5/4.png)

- ### **5.3. Установка маршрута по-умолчанию.**

- **Меняем настройки netplan на каждой машине ws и сразу смотрим результаты**

![Снова менять netplan, урааа](img5/ws112122.png)

- **Вызвать ip r и показать, что добавился маршрут в таблицу маршрутизации**

![ipr](img5/iprws.png)

- **Пропинговать с ws11 роутер r2 и показать на r2, что пинг доходит. Для этого использовать команду:** `tcpdump -tn -i eth1`

- ### **5.4. Добавление статических маршрутов.**

- **Добавить в роутеры r1 и r2 статические маршруты в файле конфигураций.**

![r1 static](img5/r1s.png)
![r2 stitic](img5/r2s.png)

- **Вызвать `ip r` и показать таблицы с маршрутами на обоих роутерах. Пример таблицы на r1 и r2:**

![ipr](img5/iprr1r2.png)

- **Запустить команды на ws11:**

`ip r list 10.10.0.0/[маска сети]`
`ip r list 0.0.0.0/0`

![ip0ws11](img5/ws11ip0.png)
Для адреса 10.10.0.0/[порт сети] был выбран маршрут, отличный от 0.0.0.0/0, потому что маска /18 описывает маршрут к сети точнее, в отличие от маски /0

- ### **5.5. Построение списка маршрутизаторов.**

- **Запустить на r1 команду:** `tcpdump -tnv -i eth0` 
- **При помощи утилиты traceroute построить список маршрутизаторов на пути от ws11 до ws21:**
<br>

![Traceroute1](img5/Traceroute1.png)
![Traceroute2](img5/Traceroute2.png)

Спустя более 10 часов страданий в попытках правильно сделать данный пункт задания, переписывая множество раз netplan, переустанавливая машины, и переписывая firewall, всё-таки удалось получить нужный результат. А теперь вернемся к сути.

- **Принцип работы traceroute:**

Для определения промежуточных маршрутизаторов traceroute отправляет серию пакетов данных целевому узлу, при этом каждый раз увеличивая на 1 значение поля TTL («время жизни»). Это поле обычно указывает максимальное количество маршрутизаторов, которое может быть пройдено пакетом. Первый пакет отправляется с TTL, равным 1, и поэтому первый же маршрутизатор возвращает обратно сообщение ICMP, указывающее на невозможность доставки данных. Traceroute фиксирует адрес маршрутизатора, а также время между отправкой пакета и получением ответа (эти сведения выводятся на монитор компьютера). Затем traceroute повторяет отправку пакета, но уже с TTL, равным 2, что позволяет первому маршрутизатору пропустить пакет дальше.

Процесс повторяется до тех пор, пока при определённом значении TTL пакет не достигнет целевого узла. При получении ответа от этого узла процесс трассировки считается завершённым.

- ### **5.6. Построение списка маршрутизаторов.**

- **Запустить на r1 перехват сетевого трафика, проходящего через eth0 с помощью команды:**
`tcpdump -n -i eth0 icmp`
- **Пропинговать с ws11 несуществующий IP (например, *10.30.0.111*) с помощью команды:**
`ping -c 1 10.30.0.111`

![5.6](img5/56.png)
<br>

## Part 6. Динамическая настройка IP с помощью **DHCP**

- **Для r2 настроить в файле /etc/dhcp/dhcpd.conf конфигурацию службы DHCP:**
<br>

- **1) указать адрес маршрутизатора по-умолчанию, DNS-сервер и адрес внутренней сети.**
![DNS-сервер и адрес внутренней сети](img6/61.png)

Устанавливаем DHCP сервер командой `sudo apt-get install isc-dhcp-server`

- **2) в файле *resolv.conf* прописать `nameserver 8.8.8.8.`**

![resolv.conf](img6/62.png)

- **Перезагрузить службу DHCP командой systemctl restart isc-dhcp-server. Машину ws21 перезагрузить при помощи reboot и через ip a показать, что она получила адрес. Также пропинговать ws22 с ws21.**

![ws22 с ws21](img6/63.png)
 - **Вывод команды `ip a` на ws21**
 
!['ip a' на ws21](img6/64.png)

 - **Пинг с ws22 к ws21**

![Пинг с ws22 к ws21](img6/65.png)

 - **Указать MAC адрес у ws11, для этого в `etc/netplan/00-installer-config.yaml` надо добавить строки: `macaddress: 10:10:10:10:10:BA`, `dhcp4: true`**

 ![MAC адрес у ws11](img6/66.png)

  - **Для r1 настроить аналогично r2, но сделать выдачу адресов с жесткой привязкой к MAC-адресу (ws11). Провести аналогичные тесты**
  - `/etc/dhcp/dhcpd.conf` для машины r1
   ![/etc/dhcp/dhcpd.conf](img6/67.png)
  - `/etc/resolv.conf` для машины r1
   ![/etc/resolv.conf](img6/68.png)
  - Перезагрузка службы DHCP командой `systemctl restart isc-dhcp-server` на r1 и reboot ws11.
   ![systemctl restart isc-dhcp-server](img6/69.png)
  - `ip a` на ws11
   !['ip a' на ws11](img6/610.png)

   - **Запросить с ws21 обновление ip адреса**

  -  `ip a` на ws21 до
  ![ws21 до](img6/611.png)
  -  `ip a` на ws21 после
  ![ws21 после](img6/612.png)

- `dhclient -r` для сброса текущего адреса
- `dhclient -v` для получения нового адреса

## Part 7. **NAT**

`sudo apt install apache2`

##### В файле */etc/apache2/ports.conf* на ws22 и r1 изменить строку `Listen 80` на `Listen 0.0.0.0:80`, то есть сделать сервер Apache2 общедоступным
<br>

- **ws22**
![apache ws22](img7/71.png)
<br>

- **r1**
![apache r1](img7/72.png)
- **Запустить веб-сервер Apache командой `service apache2 start` на ws22 и r1**

- **ws22**
![apache start ws22](img7/73.png)
<br>

- **r1**
![apache start r1](img7/74.png)

- **Добавить в фаервол, созданный по аналогии с фаерволом из Части 4, на r2 следующие правила:**
- **1) Удаление правил в таблице filter - `iptables -F`**
- **2) Удаление правил в таблице "NAT" - `iptables -F -t nat`**
- **3) Отбрасывать все маршрутизируемые пакеты - `iptables --policy FORWARD DROP`**

![firewall r2 write](img7/75.png)
- **Запускаем firewall**

![Запускаем firewall r2](img7/76.png)

- **Проверить соединение между ws22 и r1 командой `ping`**

![ping r1ws22](img7/79.png)

Не успех!

**Добавить в файл ещё одно правило**
- **4) Разрешить маршрутизацию всех пакетов протокола ICMP**
**Запускать файл также, как в Части 4**

![firewall r2 write2](img7/710.png)
![Запускаем firewall r22](img7/711.png)

- **Проверить соединение между ws22 и r1 командой `ping`**

![ping r1ws222](img7/713.png)
Успех!

**Добавить в файл ещё два правила:**
- **5) Включить SNAT, а именно маскирование всех локальных ip из локальной сети, находящейся за r2 (по обозначениям из Части 5 - сеть 10.20.0.0)**
- **6) Включить DNAT на 8080 порт машины r2 и добавить к веб-серверу Apache, запущенному на ws22, доступ из вне сети**

![firewall r2 write3](img7/777.png)

**Запускаем**

![Запускаем firewall r23](img7/778.png)

*Перед тестированием рекомендуется отключить сетевой интерфейс **NAT** (его наличие можно проверить командой `ip a`) в VirtualBox, если он включен*

- **Проверить соединение по TCP для SNAT, для этого с ws22 подключиться к серверу Apache на r1 командой:**

`telnet [адрес] [порт]`

![telnet ws22](img7/tel.png)

- **Проверить соединение по TCP для **DNAT**, для этого с r1 подключиться к серверу Apache на ws22 командой `telnet` (обращаться по адресу r2 и порту 8080)**

![telnet r1](img7/tel2.png)

## Part 8. Дополнительно. Знакомство с **SSH Tunnels**
<br>

**Запустить на r2 фаервол с правилами из Части 7**

![firewall r2 write4](img8/81.png)
![Запускаем firewall r24](img8/82.png)

**Запустить веб-сервер **Apache** на ws22 только на localhost (то есть в файле */etc/apache2/ports.conf* изменить строку `Listen 80` на `Listen localhost:80`)**

![apache ports ws22](img8/83.png)

**Воспользоваться *Local TCP forwarding* с ws21 до ws22, чтобы получить доступ к веб-серверу на ws22 с ws21 `ssh -L local_port:remote_ip:remote_port user@hostname`**

![tunelws21ws22](img8/84.png)

**Воспользоваться *Local TCP forwarding* с ws11 до ws22, чтобы получить доступ к веб-серверу на ws22 с ws11 `ssh -L local_port:remote_ip:remote_port user@hostname`**

![tunelws11ws22](img8/85.png)

**Для проверки, сработало ли подключение в обоих предыдущих пунктах, перейдите во второй терминал (например, клавишами Alt (option на Mac OS) + F2) и выполните команду: `telnet 127.0.0.1 [локальный порт]`**

![telnetws11](img8/86.png)
![telnetws22](img8/87.png)
