# Домашнее Задание № 4 ZFS
## Подготовка стенда 
  Для подготовки стенда был использован vagrant файла ДЗ №3 и исправлен. Добавил  6 одинаковых дисков по 512 мегабайт. В результате получил 8 одинаковых дисков. 

### 1. Определение алгоритма с наилучшим сжатием

    lsblk
Создаём пулы из двух дисков в режиме RAID 1:

    zpool create otus2 mirror /dev/sdd /dev/sde
    zpool create otus2 mirror /dev/sdd /dev/sde
    zpool create otus3 mirror /dev/sdf /dev/sdg
    zpool create otus4 mirror /dev/sdh /dev/sdi

Смотрим информацию о пулах: 

    zpool list

Добавим разные алгоритмы сжатия в каждую файловую систему:

    zfs set compression=lzjb otus1
    zfs set compression=lz4 otus2
    zfs set compression=gzip-9 otus3
    zfs set compression=zle otus4

Скачаем один и тот же текстовый файл во все пулы:

     for i in {1..4}; do wget -P /otus$ihttps://gutenberg.org/cache/epub/2600/pg2600.converter.log; done

проверим степень сжатия файлов:

    zfs list

    NAME             USED  AVAIL  REFER  MOUNTPOINT
    otus1           24.1M   344M  21.5M  /otus1
    otus2           17.7M   350M  17.6M  /otus2
    otus3           10.8M   357M  10.7M  /otus3
    otus4           39.1M   329M  39.1M  /otus4


    zfs get all | grep compressratio | grep -v ref

    otus1              compressratio         1.78x                  -
    otus2              compressratio         2.22x                  -
    otus3              compressratio         3.64x                  -
    otus4              compressratio         1.00x                  -

Получается что алгоритм gzip-9 самый эффективный по сжатию.

##  2. Определение настроек пула

Скачиваем архив в домашний каталог:
    wget -O archive.tar.gz --no-check-certificate 'https://drive.google.com/open?id=1KRBNW33QWqbvbVHa3hLJivOAt60yukkg&xport=download'

Разархивируем его:
    tar -xzvf archive.tar.gz

попытка импортировать пулл провалилась из за несовпадения версий, поэтому импортируем только в readonly режиме:

    zpool import otus -o readonly=on -d /root/zpoolexport/

    [root@lvm ~]# zpool status otus
      pool: otus
    state: ONLINE
      scan: none requested
    config:

            NAME                         STATE     READ WRITE CKSUM
            otus                         ONLINE       0     0     0
              mirror-0                   ONLINE       0     0     0
                /root/zpoolexport/filea  ONLINE       0     0     0
                /root/zpoolexport/fileb  ONLINE       0     0     0
    errors: No known data errors
Запрос сразу всех параметром файловой системы или на выбор : 
    zfs get all otus
    zfs get available otus
    zfs get readonly otus
    zfs get recordsize otus
    zfs get compression otus
    zfs get checksum otus

### 3. Работа со снапшотом, поиск сообщения от преподавателя

скачиваем файл 

     wget -O otus_task2.file --no-check-certificate https://drive.google.com/u/0/uc?id=1gH8gCL9y7Nd5Ti3IRmplZPF1XjzxeRAG

 Попытка восстановить в пул otus провалилась так как он только для чтения 
 zfs receive otus/test@today < otus_task2.file
импортируем в другой пул 
cat otus_task2.file | sudo zfs recv pool1/task3

    zfs list
    NAME             USED  AVAIL  REFER  MOUNTPOINT
    otus            2.03M   350M    24K  /otus
    otus/hometask2  1.88M   350M  1.88M  /otus/hometask2
    otus1           24.1M   344M  21.5M  /otus1
    otus1/task3     2.46M   344M  2.44M  /otus1/task3
    otus2           17.7M   350M  17.6M  /otus2
    otus3           10.8M   357M  10.7M  /otus3
    otus4           39.1M   329M  39.1M  /otus4

Ищем файл в каталоге и читаем его:

    find /otus1/task3 -name "secret_message"
    /otus1/task3/task1/file_mess/secret_message
    cat /otus1/task3/task1/file_mess/secret_message
    https://github.com/sindresorhus/awesome


