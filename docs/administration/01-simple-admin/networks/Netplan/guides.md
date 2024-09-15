# Рекомендуемые сопособы решения стандартных задач

Ниже перечислены основные задачи, которые можно решить при помощи Netplan. В [этом](https://github.com/canonical/netplan/tree/main/examples) репозитории можно найти больше образцов.

## Использование DHCP и статических адрсов

Допустим, у нас есть интерфес `enp3s0`, который получает адрес при помощи DHCP сервера. Конфиг для него будет выглядеть так:

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp3s0:
      dhcp4: true
```

Усложним задачу и установим вместо этого статический адрес. Будем использовать ключ `addresses`, который позволит установить список адресов (да, на один интерфейс мы сожем посадить несколько адресов). При этом адреса должны записываться с маской подсети (CIDR). Также нужно предоставить информацию о серверах DNS и прописать **шлюз в виде маршрута по умолчнию**:

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp3s0:
      addresses:
        - 10.10.10.2/24
      nameservers:
        search: [mydomain, otherdomain]
        addresses: [10.10.10.1, 1.1.1.1]
      routes:
        - to: default
          via: 10.10.10.1
```

## Подключение нескольких интерфейсов по DHCP

Нектороые современные системы позволяют использовать больше одного сетевого интерфейса. Обычно сервера нуждаются в подключении через несколько интерфейсов и могут принимать трафик из интернета только через назначенный интерфейс, несмотря на то, что все сетевые порты подключены к одному, вполне исправному, шлюзу.

Точной маршрутизации можно добиться и при обращении к DHCP серверу, указав метрики для маршрутов, которые идут через DHCP. Эти метрики будут гарантировать, что некоторое количество маршрутов будет более привелегированным по сравнению с другими. Чем меньшее число, тем более привелегированным является маршрут.

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp1s0:
      dhcp4: yes
      dhcp4-overrides:
        route-metric: 100
    enp2s0:
      dhcp4: yes
      dhcp4-overrides:
        route-metric: 200
```

Здесь более привелегированным является `enp1s0`

## Подключение к открытым беспроводным сетям

Netplan легко настроить на подключение к открытому Wi-Fi (например). Достаточно просто прописать список, либо оставить его пустым.

```yaml
network:
  version: 2
  renderer: networkd
  wifis:
    wlp1s0:
      access-points:
        opennetwork: {}
      dhcp4: yes
```

## Подключение к персональным беспроводным сетям (WPA)

Устройства, перечисленные в ключе "wifis" конфигурируются подобно устройствам "ethernets". Дополнительно нужно только указать имя беспроводной сети (SSID) и пароль:

```yaml
network:
  version: 2
  renderer: networkd
  wifis:
    wlp1s0:
      dhcp4: no
      dhcp6: no
      addresses: [192.168.0.21/24]
      nameservers:
        addresses: [192.168.0.1, 8.8.8.8]
      access-points:
        "Network_SSID":
          password: "xxxxx"
      routes:
        - to: default
          via: 192.168.0.1
```

## Подключение к беспроводным сетям организаций (WPA Enterprise)

Нередко в таких сетях используется WPA или WPA2 Enterprise, которые запрашивают дополнительные параметры аутентификации. Ниже показан пример подключения к сети, использующей WPA-EAP и TTLS:

```yaml
network:
  version: 2
  renderer: networkd
  wifis:
    wlp1s0:
      access-points:
        workplace:
          auth:
            key-managment: eap
            method: ttls
            anonymous-identity: "@internal.example.com"
            identity: "joe@internal.example.com"
            password: "xxxxxx"
      dhcp4: yes
```

Или WPA-EAP и TLS:

```yaml
network:
  version: 2
  renderer: networkd
  wifis:
    wlp1s0:
      access-points:
        university:
          auth:
            key-managment: eap
            method: tls
            anonymous-identity: "@cust.example.com"
            identity: "cert-joe@cust.example.com"
            ca-certificate: /etc/ssl/cust-cacrt.pem
            client-certificate: /etc/ssl/cust-crt.pem
            client-key: /etc/ssl/cust-key.pem
            client-key-password: "xxxxxxxx"
      dhcp4: yes
```

## Использование нескольких адресов на одном интерфейсе

Можно передат список адресов и даже с параметрами. Но эта возможность есть только у systemd-networkd.

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp3s0:
      addresses:
        - 10.100.1.37/24
        - 10.100.1.38/24:
            label: "enp3s0:0"
        - 10.100.1.39/24:
            label: "enp3s0:some-label"
      routes:
        - to: default
          via: 10.100.1.1
```

## Использование нескольких адрессов с несколькими шлюзами

Данный вариант похож на представлнный выше. Интерфейсы с несколькими адресамимогут конфигурироваться с несколькими шлюзами. Нужно только указать DNS сервера.

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp3s0:
      addresses:
        - 10.0.0.10/24
        - 10.0.0.11/24
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
      routes:
        - to: default
          via: 10.0.0.1
          metric: 200
        - to: default
          via: 11.0.0.1
          metric: 300
```

Мы отдельным образом конфигурируем маршруты до назначения по умолчанию (или 0.0.0.0/0) используя адреса шлюзов для подсетей. Ключ `metric` нужно скорректировать таким образом, чтобы маршрутизация шла должным образом.

DHCP можно использовать для назначения адресов одному из набора интерфейсов. В этом случае маршрут по умолчанию для этого адреса будет автоматически конфигурироваться с метрикой 100.

## Настройка агрегации интерфейсов

Бондинг конфигурируется в секции `bonds` как список физических интерффейсов и указания вида обработки сигналов.

```yaml
network:
  version: 2
  renderer: networkd
  bonds:
    bond0:
      dhcp4: yes
      interfaces:
        - enp1s0
        - enp2s0
      parameters:
        mode: active-backup
        primary: enp1s0
```

Ниже показан пример системы, в которой маршрутизация производится с рядом агрегированных интрефейсов и с различнми типами обработки. Отметка ‘optional: true’ декларирует, что можно производить загрузку ОС не дожидаясь того момента, пока вся конфигурация будет поднята.

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp1s0:
      dhcp4: no
    enp2s0:
      dhcp4: no
    enp3s0:
      dhcp4: no
      optional: true
    enp4s0:
      dhcp4: no
      optional: true
    enp5s0:
      dhcp4: no
      optional: true
    enp6s0:
      dhcp4: no
      optional: true
  bounds:
    bond-lan:
      interfaces: [enp2s0, enp3s0]
      addresses: [192.168.93.2/24]
      parameters:
        mode: 802.3ad
        mii-monitor-interval: 1
    bond-wan:
      interfaces: [enp1s0, enp4s0]
      addresses: [192.168.1.252/24]
      nameservers:
        search: [local]
        addresses: [8.8.8.8, 8.8.4.4]
      parameters:
        mode: active-backup
        mii-monitor-interval: 1
        gratuitious-arp: 5
      routes:
        - to: default
          via: 192.168.1.1
    bond-conntrack:
      interfaces: [enp5s0, enp6s0]
      addresses: [192.168.254.2./24]
      parameters:
        mode: balance-rr
        mii-monitor-interval: 1
```

## Настройка сетевых мостов

Для создания и самого простого моста состоящего из единичных устройств которые используют DHCP, мы напишем следующее:

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp3s0:
      dhcp4: no
  bridges:
    br0:
      dhcp4: yes
      interfaces:
        - enp3s0
```

Для более полного примера допустим, что у нас имеется виртуальная машина на libvirtd и мы используем её в отдельном VLAN'е через мост, чтобы обеспечить интегрированный интерфейс для неё.

> _Прим._: мост в таком случае нужен между виртуальной сетью, которую создаёт гипервизор внутри хостовой машины и физической, к которой подключена хостовая машина

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s25:
      dhcp4: true
  bridges:
    br0:
      addresses: [10.3.99.25/24]
      interfaces: [vlan15]
  vlans:
    vlan15:
      accept-ra: no
      id: 15
      link: enp0s25
```

Теперь libvirtd может использовать мост как добавляемый и отслеживаемый контент. Можно в `/etc/libvirtd/qemu/networks/` прописать XML. В нём в теге `<network>` в теге `<name>` прописать записанное в конфигурации Netplan имя моста, как и в теге `<bridge>` в атрибуте `name`:

```xml
<network>
    <name>br0</name>
    <bridge name='br0'/>
    <forward mode="bridge"/>
</network>
```

## Прикрепление VLAN'ов к сетевым интерфейсам

Для конфигурирования нескольких VLAN'ов с переименованнми интерфесами можно использовать такой YAML:

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    mainif:
      match:
        macaddress: "de:ad:be:ef:ca:fe"
      set-name: mainif
      addresses: [10.3.0.5/23]
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
        search: [example.com]
      routes:
        - to: default
          via: 10.3.0.1
  vlans:
    vlan15:
      id: 15
      link: mainif
      addesses: [10.3.99.5/24]
    vlan10:
      id: 10
      link: mainif
      addresses: [10.3.98.5/24]
      nameservers:
        addresses: [127.0.0.1]
        search: [domain1.example.com, domain2.example.com]
```

## Работа с прямыми маршрутами

Это позволяет настроить любой маршрут, но используется для настройки маршрута по умолчанию. Для использования этого свойства, нужно использовать ключевое слово `on-link` (_boolean_). Используется даже когда IP-адрес, к которому идёт подключение, не содержит составляющей с информацией о подсети.

```yaml
network:
  version: 2
  renderer: networkd
    ethernets:
      ens3s0:
        addresses: [ "10.10.10.1/24" ]
          routes:
            - to: default # or 0.0.0.0/0
              via: 9.9.9.9
              on-link: true
```

## Настройка исходящей маршрутизации

В таблицу маршрутизации можно добавлять особые интерфейсы для обеспечения маршрутизации между двумя сетями.

Например, ниже показаны интерфейсы `ens3` с адресом сети `192.168.3.0/24` и `ens5` с адресом сети `192.168.5.0/24`. Это позволяет клиентам одной сети подключаться к другой сети и получать оттуда ответ с правильного интерфейса.
Более того, маршрут по умолчанию всё ещё назначен `ens5`. Он позволяет любому другому трафику проходить через него.

```yaml
network:
  version: 2
  renderer: networkd
    ethernets:
      ens3:
        addresses: [192.168.3.30/24]
        dhcp4: no
        routes:
          - to: 192.168.3.0/24
            via: 192.168.3.1
            table: 101
        routing-policy:
          - from: 192.168.3.0/24
            table: 101
      ens5:

```
