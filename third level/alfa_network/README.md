[Вернуться к оглавлению](https://github.com/engine-it-in/different-level-task/blob/main/README.md)
***
* [4 части сети](#4-части-сети)
  * [k8s](#k8s)
  * [DNS](#dns)
***

# 4 части сети

- Интернет (клиенты)
- DMZ зона 10.x (демилитаризованная сетевая зона) с запрещающими политиками безопасности (с доп.мониторингами)
    - тут у нас есть только балансировщики (reverse-proxy), которые приземляет на prelive.wapi.vesta.ru или
      production.wapi.vesta.ru
    - защита от ddos
- ==================== FireWall ====================
    - проверка трафика (можно ли его пустить в LAN)
- ==================== Q-Rator ====================
- LAN (172.16.x.x)
- Yandex облако (виртуальная сеть для нас)

## k8s
ping production.wapi.vesta.ru
ping prelive.wapi.vesta.ru
мастер-нода только оркестрирует

## DNS

- внешний - через интернет
- внутренний - внутри сети