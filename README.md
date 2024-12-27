# redis-cluster-with-sentinel
Инструкция по настройке кластера Redis с Sentinel для автоматического переключения мастера, интеграцией с Keepalived для обеспечения доступности виртуального IP-адреса Haproxy, а также использованием Docker Compose для развёртывания.

Для настройки нам понадобится 5 вм. 
3 вм с redis и sentinel. 2 вм с haproxy

1. Node-1 (Redis Master) 

1.1 Копируем файл composes/docker-compose-redis-master.yml 
1.2 Копируем файл composes/docker-compose-sentinel-1.yml
1.3 Cоздаем волюм redis_data - docker volume create redis_data 
1.4 Запускаем контейнеры

2 Node-2 (Redis slave 1)

2.1 Копируем файл composes/docker-compose-redis-slave-1.yml 
2.2 Копируем файл composes/docker-compose-sentinel-2.yml
2.3 Cоздаем волюм redis_data - docker volume create redis_data
2.4 Запускаем контейнеры


3. Node-3 (Redis slave 2)

3.1 Копируем файл composes/docker-compose-redis-slave-2.yml 
3.2 Копируем файл composes/docker-compose-sentinel-3.yml
3.3 Cоздаем волюм redis_data - docker volume create redis_data
3.4 Запускаем контейнеры


4. Node-4 (Haproxy main)

4.1 Устанавливаем haproxy 
4.2 В конец /etc/haproxy/haproxy.cfg добавляем содержимое ./haproxy/haproxy-cfg-main.cfg
4.3 Перезапускаем haproxy - systemctl restart haproxy
4.4 Устанавливаем keepalived -  apt install keepalived
4.5 В /etc/keepalived/keepalived.conf вставляем конфиг ./keepalived/keepalived-main.conf
4.6 Перезапускаем keepalived -  systemctl restart keepalived

5. Node-5 (Haproxy slave)

5.1 Устанавливаем haproxy 
5.2 В конец /etc/haproxy/haproxy.cfg добавляем содержимое ./haproxy/haproxy-cfg-slave.cfg
5.3 Перезапускаем haproxy - systemctl restart haproxy
5.4 Устанавливаем keepalived -  apt install keepalived
5.5 В /etc/keepalived/keepalived.conf вставляем конфиг ./keepalived/keepalived-slave.conf
5.6 Перезапускаем keepalived -  systemctl restart keepalived


Очень важно настроить правильное время на серверах