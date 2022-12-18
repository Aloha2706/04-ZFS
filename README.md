# Домашнее Задание № 4 ZFS
## Подготовка стенда 
  Для подготовки стенда был использован vagrant файла ДЗ №3 и исправлен. Добавил  6 одинаковых дисков по 512 мегабайт. В результате получил 8 одинаковых дисков. 

###1. Определение алгоритма с наилучшим сжатием

    lsblk
Создаём пулы из двух дисков в режиме RAID 1:

    zpool create otus2 mirror /dev/sdd /dev/sde
    zpool create otus2 mirror /dev/sdd /dev/sde
    zpool create otus3 mirror /dev/sdf /dev/sdg
    zpool create otus4 mirror /dev/sdh /dev/sdi

Смотрим информацию о пулах: 

    zpool list

Вывод команды:

    NAME    SIZE  ALLOC   FREE  EXPANDSZ   FRAG    CAP  DEDUP  HEALTH  ALTROOT
    otus1   496M  24.1M   472M         -     4%     4%  1.00x  ONLINE  -
    otus2   496M  17.7M   478M         -     4%     3%  1.00x  ONLINE  -
    otus3   496M  10.8M   485M         -     1%     2%  1.00x  ONLINE  -
    otus4   496M  39.2M   457M         -     5%     7%  1.00x  ONLINE  -







и дописать в Vagrant файл:

    #текст выделенный строками 

вдало
# одна решетка 
##  Две решетки 
### три решетки


    hkhkhjkhkjk

jhgjhgkjhgkjhgkhj



