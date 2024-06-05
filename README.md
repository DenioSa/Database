# Домашнее задание к занятию "`ELK`" - `Сайфиев Денис`


### Задание 1 Elasticsearch

Установите и запустите Elasticsearch, после чего поменяйте параметр cluster_name на случайный.
Приведите скриншот команды 'curl -X GET 'localhost:9200/_cluster/health?pretty', сделанной на сервере с установленным Elasticsearch. Где будет виден нестандартный cluster_name.


### Решение 1



### Задание 2. Kibana

Установите и запустите Kibana.
Приведите скриншот интерфейса Kibana на странице http://<ip вашего сервера>:5601/app/dev_tools#/console, где будет выполнен запрос GET /_cluster/health?pretty.

### Решение 2

![status memcached](https://github.com/DenioSa/KRM/blob/952935ac2aebfb2df5adcf78f71cbca8e10b57ab/img/memcached.bmp)


### Задание 3. Logstash

Установите и запустите Logstash и Nginx. С помощью Logstash отправьте access-лог Nginx в Elasticsearch.

Приведите скриншот интерфейса Kibana, на котором видны логи Nginx.

### Решение 3

![key memcached](https://github.com/DenioSa/KRM/blob/69eca355f3c477c33bfbda9c16075d962353a6da/img/memcached_key.bmp)


### Задание 4. Filebeat.

Установите и запустите Filebeat. Переключите поставку логов Nginx с Logstash на Filebeat.
Приведите скриншот интерфейса Kibana, на котором видны логи Nginx, которые были отправлены через Filebeat.


### Решение 4

![key redis](https://github.com/DenioSa/KRM/blob/bb4c42e9eed09392f9225e88ed269e728cc95f60/img/redis_cli.bmp)

---

