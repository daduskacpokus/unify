# WiFi standalone controller
## Contents
### Что делаем?
- одну общую сеть WiFi на офис Бузника
- доступ в сеть осуществляется по паролю
- пароль меняется 1 раз в 2 недели
- бесшовность позволяет провести разговор по Skype идя по офису


### Для чего?

Если в двух словах - уменьшить сложность архитектуры сервиса, избавившись, по возможности, от ненужных дополнительных точек отказа:

- [x] радиус авторизация пользователей

- [x] географическая удалённость виртуальной машины контроллера (hetzner) от самих уст-в, работа которых зависит от наличия связянности с ним 

Философия «Дзен Питона»:

- *Явное лучше, чем неявное.*
- *Простое лучше, чем сложное.*
- *Встретив двусмысленность, отбрось искушение угадать.*
- *При этом практичность важнее безупречности.*

### Что получаем?

- physical-node контроллер размещенный в серверной офиса
- точки доступа имеющие связнность с  контроллером на L2 уровне (без VPN over Public Internet)
- авторизация в сети WiFi через сканирование QR-кода


### Как делаем?

- Все тарелки *Ubiquiti* расположенные по офису соединяются с контроллером через один широковещательный домен. На [изображении](https://github.com/daduskacpokus/unifi/blob/master/img/1551786772429.JPEG) L2-свич с портами №2,4,6 в которые подключаются два кабеля от тарелок, третий от контроллера
- контроллер имеет два порта **GigabitEthernet** один из которых *смотрит* в сторону домена c тарелками `intranet_iface`, а другой в интернет `internet_iface`. Хост-система контроллера работает в режиме маршрутизатора пересылая пакеты из домена тарелок (пользователей WiFi) в сеть интернет.
- В случае, если `internet_iface` для выхода в интернет использует корпоративную сеть, а не выделеный канал - пакеты из сети WiFi в частную сеть не пропускает [файрвол](https://github.com/daduskacpokus/unifi/blob/master/playbooks/roles/router-host/tasks/main.yml)
- Общий пароль для входа в WiFi сеть выдавать в виде [QR-кода](https://qrcode.tec-it.com/ru/WiFi) с названием сети и паролем достаточной надежности

### План дествий?

- [x] монтаж 1U сервера в стойку `DONE`
- [x] перевод сервера в режим роутера `DONE`
- [x] установка/настройка Unifi `DONE`
- [ ] настройка широковещательного домена в коммутационном шкафу  `PENDING`
- [ ] переключение тарелок в созданный VLAN  `PENDING`
- [ ] комутация тарелок с контроллером  `PENDING`

### Примечание

Работа WiFi по предложенной схеме уже организована на 3-ем этаже
