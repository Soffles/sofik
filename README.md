# sofik
Для начала мы устанавливаем Linux Oracle на VirtualBox, а что бы это сделать необходимо:

Иметь Linux Выделить 2+ ядер, выбрать 4096+ МБ оперативы и при установки операционной системы выбираем английский язык.
Теперь начинаем работать и вводим команды:

1 - `sudo yum install wget` 

этот код устанавливает утилиту wget.
![image](https://github.com/user-attachments/assets/5d99b507-3d65-4e14-a6ec-f33a1c3017d2)

2 - `sudo wget -P /etc/yum.repos.d/ https://download.docker.com/linux/centos/docker-ce.repo`

с помощью этого кода мы скачиваем файл репозитория
![image](https://github.com/user-attachments/assets/49a3b379-9308-4158-ad95-4750834271fe)

3 - `sudo yum install docker-ce docker-ce-cli containerd.io`

docker
![image](https://github.com/user-attachments/assets/4ab268c1-0047-4747-ab34-e92b518ff72a)

4 - `sudo systemctl enable docker --now`
Запуск и разрешаем автозапуск

5 - `sudo yum install curl`
убеждаемся в наличие пакета curl

6 - `COMVER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d\" -f4)`

Переменная COMVER - это результат curl запроса ( в ней номер последней версии Docker Compose)
![image](https://github.com/user-attachments/assets/5b086411-24a3-4b10-a7cd-0e4eeca64a12)
![image](https://github.com/user-attachments/assets/c5aa773c-044a-4afc-a54d-38575c91d193)

7 - `sudo curl -L "https://github.com/docker/compose/releases/download/$COMVER/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose`

Скачиваем скрипт docker-compose последней версии и помещаем его в каталог /usr/bin
![image](https://github.com/user-attachments/assets/7bd45598-3475-4d13-a9d3-26d7a7f46511)

8 - `sudo chmod +x /usr/bin/docker-compose`

Права на выполнение файла docker-compose
![image](https://github.com/user-attachments/assets/6a8f8417-a3c1-4d1e-bf04-79b546a05ed8)

9 - `docker-compose --version`

У нас установлена версия Docker Compose.
![image](https://github.com/user-attachments/assets/2a545530-9c81-436a-ba7e-de1be279474c)

10 - `git clone https://github.com/skl256/grafana_stack_for_docker.git`

Нам выдают ошибку, после чего предлагают скачать git. Мы спокойно даем согласие и продолжаем
![image](https://github.com/user-attachments/assets/ca37bb1f-b650-4387-ab17-adefbb5cd9b9)
![image](https://github.com/user-attachments/assets/e8ce385a-6451-44e0-b209-f0626093b5aa)

11 - `cd grafana_stack_for_docker`

Переходим в папку.
![image](https://github.com/user-attachments/assets/3b05ae98-94c4-49c0-bdac-7e84d9409232)

12 - `sudo mkdir -p /mnt/common_volume/swarm/grafana/config`
Данная команда создаёт путь /mnt/common_volume/swarm/grafana/config

13 - `sudo mkdir -p /mnt/common_volume/grafana/{grafana-config,grafana-data,prometheus-data}`

Создаётся структура каталогов для Grafana. Связанны с ней компоненты, если, конечно, они ещё не существуют.
![image](https://github.com/user-attachments/assets/02f883ee-cda5-42d0-9285-1d8cb5dc9f33)

14 - `sudo chown -R $(id -u):$(id -g) {/mnt/common_volume/swarm/grafana/config,/mnt/common_volume/grafana}`

Эти файлы и каталоги будут переданы в собственность текущему пользователю
![image](https://github.com/user-attachments/assets/39431cb6-8bbe-435f-9761-0e7919fe7340)

15 - `touch /mnt/common_volume/grafana/grafana-config/grafana.ini`
grafana.ini у нас существует. Если файл не существует, команда создаст новый пустой файл с указанным именем по указанному пути.

16 - `cp config/* /mnt/common_volume/swarm/grafana/config/`

С помощью этой команды мы копируем все файлы и подкаталоги из config в директорию /mnt/common_volume/swarm/grafana/config/
![image](https://github.com/user-attachments/assets/caf5d183-35f8-4b93-a806-d0d5a0d35f5a)

17 - `mv grafana.yaml docker-compose.yaml`

Файл grafana.yaml переименовывается в docker-compose.yaml. Правда, мы не увидим ничего.
![image](https://github.com/user-attachments/assets/fd29a713-63ba-4c28-9ba9-4289b50fe6e9)

18 - `sudo docker compose up -d`

Запускаеются контейнеры в фоновом режиме, потому что испоьзуется конфигурация из файла docker-compose.yml.
![image](https://github.com/user-attachments/assets/1f4c545b-fd2a-4501-81cb-6efde87280f5)

19 - `sudo vi docker-compose.yaml`

Перед нами файл docker-compose.yaml. После нас перекинет в текстовый редактор, где мы нажимаем insert. Для сохранения мы пишем Esc, а затем :wq! Затем node-exporter после services
![image](https://github.com/user-attachments/assets/d363e965-9e32-4348-8de6-c719fade3f15)

20 - `sudo vi prometheus.yaml`

Файл prometheus.yaml в текстовом редакторе vi с правами суперпользователя.
/mnt/common_volume/swarm/grafana/config/prometheus.yaml - необходимо поменять targets: на exporter:9100,
![image](https://github.com/user-attachments/assets/f2cb1bb3-ee18-46d3-8594-2dbe7c06439d)

# GRAFANA
Итак. Открываем сайт `localhost:3000`
- User & Password GRAFANA: `admin`
- Код: `3000`
- Код прометеуса: `http://prometheus:9090`
  
  Далее. Выбираем Dashboards и создаем Dashboard
- Кнопка +Add visualization и "Configure a new data source"
- Prometheus
- Connection
- `http://prometheus:9090`
  
  Теперь - Authentication и Basic authentication
- User: `admin`
- Password: `admin`
- Нажимаем на Save & test и мы видим зелёную галочку
  
  После => выбираем Dashboards и создаем Dashboard
- ждем кнопку "Import dashboard"
- Find and import dashboards for common applications at grafana.com/dashboards: 1860 //ждем кнопку Load
- Select Prometheus ждем кнопку "Import"
  ![image](https://github.com/user-attachments/assets/e24d84ef-c301-427e-9303-41bafb3a2af5)

  # VictoriaMetrics
  Первым делом мы изменяем docker-compose.yaml

 1 - `cd grafana_stack_for_docker`
  
  Данная команда изменяет текущий рабочий каталог на каталог grafana_stack_for_docker.

  2 - `sudo vi docker-compose.yaml`

  Открывается файл docker-compose.yaml, где в редакторе vi с правами суперпользователя.
  В самом текстовом редакторе после prometheus вставляем следующее:
  ![image](https://github.com/user-attachments/assets/603e0b7f-75c6-4637-b445-a0b580df9f11)
  
  После того, как мы вошли в connection, там, где мы писали http//:prometheus:9090, мы пишем http:victoriametrics:9090, после чего заменяем имя из "Prometheus-2" в "Vika" нажимаем на dashboards add visualition выбираем "Vika" снизу меняем на "code". И вот мы в терминале. Пишем:

  `echo -e "# TYPE OILCOINT_metric1 gauge\nOILCOINT_metric1 0" | curl --data-binary @- http://localhost:8428/api/v1/import/prometheus`

  Такие бинарные данные как: метрики в формате Prometheus отправляются на локальный сервер (на порту 8428)
  
   `curl -G 'http://localhost:8428/api/v1/query' --data-urlencode 'query=OILCOINT_metric1'`

- Делается запрос к API, что бы мы получили данные по метрике OILCOINT_metric1

- Команда выводит текст. Его мы можем использовать для определения метрики в формате, совместимый с Prometheus

- Команда выводит информацию о значении этой метрики в формате, который может быть использован системой мониторинга Prometheus.
  ![image](https://github.com/user-attachments/assets/c2434d09-41f4-411a-91dd-70e0976678c4)

Необходимо поменять 0 на любое другое значение. Копируем переменную OILCOINT_metric1 и вставляем в query. Теперь нажимаем кнопку RUN 
![image](https://github.com/user-attachments/assets/e0f139a7-e938-47af-adec-72eeab58c109)
![image](https://github.com/user-attachments/assets/3a81887f-75ad-43bb-8ff1-ed4ed5e18822)

Копируем OILCOINT_metric1, вставляем в code и готово
![image](https://github.com/user-attachments/assets/7009229d-a3fc-46fa-99fe-39db2c9c8dae)



  













