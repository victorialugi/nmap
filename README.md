# Домашнее задание к занятию "`Docker. Часть 2`" - `Тукаев Айрат`


### Инструкция по выполнению домашнего задания

   1. Сделайте `fork` данного репозитория к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/git-hw или  https://github.com/имя-вашего-репозитория/7-1-ansible-hw.
   2. Выполните клонирование данного репозитория к себе на ПК с помощью команды `git clone`.
   3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
      - впишите вверху название занятия и вашу фамилию и имя
      - в каждом задании добавьте решение в требуемом виде (текст/код/скриншоты/ссылка)
      - для корректного добавления скриншотов воспользуйтесь [инструкцией "Как вставить скриншот в шаблон с решением](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md)
      - при оформлении используйте возможности языка разметки md (коротко об этом можно посмотреть в [инструкции  по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md))
   4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`);
   5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
   6. Любые вопросы по выполнению заданий спрашивайте в чате учебной группы и/или в разделе “Вопросы по заданию” в личном кабинете.
   
Желаем успехов в выполнении домашнего задания!
   
### Дополнительные материалы, которые могут быть полезны для выполнения задания

1. [Руководство по оформлению Markdown файлов](https://gist.github.com/Jekins/2bf2d0638163f1294637#Code)

---

### Задание 1

Установите Docker Compose и опишите, для чего он нужен и как может улучшить лично вашу жизнь.

**Решение 1**

Docker Compose позволяет легко управлять несколькими контейнерами Docker, определенными в едином файле YAML (docker-compose.yml). Его использование позволяет мне меньше тратить времени на установку зависимых компонентов отдельно друг от друга. Все компоненты проекта собираются автоматически одним простым командным файлом.

```
sudo curl -L "https://github.com/docker/compose/releases/download/v2.18.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```
---

### Задание 2

Выполните действия и приложите текст конфига на этом этапе.
Создайте файл docker-compose.yml и внесите туда первичные настройки:
* version;  
* services;  
* volumes;   
* networks.  
При выполнении задания используйте подсеть 10.5.0.0/16. Ваша подсеть должна называться: <ваши фамилия и инициалы>-my-netology-hw. Все приложения из последующих заданий должны находиться в этой конфигурации.

**Решение 2**

```
version: '3'
services:

volumes:

networks:
  tukaevar-my-netology-hw:
    driver: bridge
    ipam:
      config:
        - subnet: 10.5.0.0/16
        
```

---

### Задание 3

1. Создайте конфигурацию docker-compose для Prometheus с именем контейнера <ваши фамилия и инициалы>-netology-prometheus.
2. Добавьте необходимые тома с данными и конфигурацией (конфигурация лежит в репозитории в директории 6-04/prometheus ).
3. Обеспечьте внешний доступ к порту 9090 c докер-сервера.

**Решение 3**

docker-compose.yml
```
version: '3'
services:
  prometheus:
    image: prom/prometheus:v3.6.0
    container_name: tukaevar-netology-prometheus
    command: --web.enable-lifecycle --config.file=/etc/prometheus/prometheus.yml
    ports:
      - 9090:9090
    volumes:
      - ./:/etc/prometheus
      - prometheus-data:/prometheus
    networks:
      - tukaevar-my-netology-hw
    restart: always

volumes:
  prometheus-data:
networks:
  tukaevar-my-netology-hw:
    driver: bridge
    ipam:
      config:
        - subnet: 10.5.0.0/16
          gateway: 10.5.0.1
```

```
docker compose up -d
```


![Скрин 1](img/img21.png)

---

### Задание 4

1. Создайте конфигурацию docker-compose для Pushgateway с именем контейнера <ваши фамилия и инициалы>-netology-pushgateway.
2. Обеспечьте внешний доступ к порту 9091 c докер-сервера.

**Решение 4**

В docker-compose добавил конфигурацию для Pushgateway.
```
 services:
  pushgateway:
    image: prom/pushgateway:v1.11.1
    container_name: tukaevar-netology-pushgateway
    ports:
      - 9091:9091
    networks:
      - tukaevar-my-netology-hw
    depends_on:
      - prometheus
    restart: unless-stopped
```

---

### Задание 5

1. Создайте конфигурацию docker-compose для Grafana с именем контейнера <ваши фамилия и инициалы>-netology-grafana.
2. Добавьте необходимые тома с данными и конфигурацией (конфигурация лежит в репозитории в директории 6-04/grafana.)
3. Добавьте переменную окружения с путем до файла с кастомными настройками (должен быть в томе), в самом файле пропишите логин=<ваши фамилия и инициалы> пароль=netology.
4. Обеспечьте внешний доступ к порту 3000 c порта 80 докер-сервера.

**Решение 5**

docker-compose.yml
```
version: '3'
services:
  prometheus:
    image: prom/prometheus:v3.6.0
    container_name: tukaevar-netology-prometheus
    command: --web.enable-lifecycle --config.file=/etc/prometheus/prometheus.yml
    ports:
      - 9090:9090
    volumes:
      - ./:/etc/prometheus
      - prometheus-data:/prometheus
    networks:
      - tukaevar-my-netology-hw
    restart: always
  pushgateway:
    image: prom/pushgateway:v1.11.1
    container_name: tukaevar-netology-pushgateway
    ports:
      - 9091:9091
    networks:
      - tukaevar-my-netology-hw
    depends_on:
      - prometheus
    restart: unless-stopped
  grafana:
    image: grafana/grafana
    container_name: tukaevar-netology-grafana
    environment:
      GF_PATHS_CONFIG: /etc/grafana/custom.ini
    ports:
      - 80:3000
    volumes:
      - ./grafana:/etc/grafana
      - grafana-data:/var/lib/grafana
    networks:
      - tukaevar-my-netology-hw
    depends_on:
      - prometheus
    restart: unless-stopped
volumes:
  prometheus-data:
  grafana-data:
networks:
  tukaevar-my-netology-hw:
    driver: bridge
    ipam:
      config:
        - subnet: 10.5.0.0/16
          gateway: 10.5.0.1
```
customm.ini
```
[security]

admin_user = tukaevar
admin_password = netology
```

---

### Задание 6

1. Настройте поочередность запуска контейнеров.
2. Настройте режимы перезапуска для контейнеров.
3. Настройте использование контейнерами одной сети.
4. Запустите сценарий в detached режиме.

**Решение 6**

Очерёдность запуска контейнеров, режимы перезапуска, использование контейнерами одной сети прописано в решении предыдушей задачи.

Запускаю сценарий в detached режиме.
```
docker compose up -d
```

![](img/img22.png)


![](img/img23.png)

---

### Задание 7

1. Выполните запрос в Pushgateway для помещения метрики <ваши фамилия и инициалы> со значением 5 в Prometheus: echo "<ваши фамилия и инициалы> 5" | curl --data-binary @- http://localhost:9091/metrics/job/netology.
2. Залогиньтесь в Grafana с помощью логина и пароля из предыдущего задания.
3. Cоздайте Data Source Prometheus (Home -> Connections -> Data sources -> Add data source -> Prometheus -> указать "Prometheus server URL = http://prometheus:9090" -> Save & Test).
4. Создайте график на основе добавленной в пункте 5 метрики (Build a dashboard -> Add visualization -> Prometheus -> Select metric -> Metric explorer -> <ваши фамилия и инициалы -> Apply.

В качестве решения приложите:

* docker-compose.yml целиком;  
* скриншот команды docker ps после запуске docker-compose.yml;  
* скриншот графика, постоенного на основе вашей метрики.  

**Решение 7**

docker-compose.yml
```
version: '3'
services:
  prometheus:
    image: prom/prometheus:v3.6.0
    container_name: tukaevar-netology-prometheus
    command: --web.enable-lifecycle --config.file=/etc/prometheus/prometheus.yml
    ports:
      - 9090:9090
    volumes:
      - ./:/etc/prometheus
      - prometheus-data:/prometheus
    networks:
      - tukaevar-my-netology-hw
    restart: always
  pushgateway:
    image: prom/pushgateway:v1.11.1
    container_name: tukaevar-netology-pushgateway
    ports:
      - 9091:9091
    networks:
      - tukaevar-my-netology-hw
    depends_on:
      - prometheus
    restart: unless-stopped
  grafana:
    image: grafana/grafana
    container_name: tukaevar-netology-grafana
    environment:
      GF_PATHS_CONFIG: /etc/grafana/custom.ini
    ports:
      - 80:3000
    volumes:
      - ./grafana:/etc/grafana
      - grafana-data:/var/lib/grafana
    networks:
      - tukaevar-my-netology-hw
    depends_on:
      - prometheus
    restart: unless-stopped
volumes:
  prometheus-data:
  grafana-data:
networks:
  tukaevar-my-netology-hw:
    driver: bridge
    ipam:
      config:
        - subnet: 10.5.0.0/16
          gateway: 10.5.0.1
```


![](img/img25.png)


![](img/img24.png)

---

### Задание 8

1. Остановите и удалите все контейнеры одной командой.  

В качестве решения приложите скриншот консоли с проделанными действиями.

**Решение 8**


![](img/img26.png)

---
