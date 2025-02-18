# Cattygram 

## Описание проекта:
Kittygram — социальная сеть для обмена фотографиями любимых питомцев.

## Инструкция по запуску:
### Запуск локально:
#### Клонируйте репозиторий:
```
git clone git@github.com:ViaDo1orosa/kittygram_final.git
```
Создайте файл `.env` и заполните его своими данными. Все необходимые переменные перечислены в файле `.env.example`, находящемся в корневой директории проекта.

#### Создание Docker-образов:

Замените `YOUR_USERNAME` на свой логин на DockerHub:

```
cd frontend
docker build -t YOUR_USERNAME/kittygram_frontend .
cd ../backend
docker build -t YOUR_USERNAME/kittygram_backend .
cd ../nginx
docker build -t YOUR_USERNAME/kittygram_gateway . 
```

#### Загрузите образы на DockerHub:

```
docker push YOUR_USERNAME/kittygram_frontend
docker push YOUR_USERNAME/kittygram_backend
docker push YOUR_USERNAME/kittygram_gateway
```

#### Установите Docker Compose:

```
sudo apt update
sudo apt install curl
curl -fSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh
sudo apt-get install docker-compose-plugin
```

#### Запустите Docker Compose с этой конфигурацией на своём компьютере:
```
docker compose -f docker-compose.production.yml up
```
#### Сразу же соберите статику, примените миграции:

```
docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. /backend_static/static/
docker compose -f docker-compose.production.yml exec backend python manage.py migrate
```
Проект будет достпен на:
```
http://localhost:9000/
```
### Запуск на удаленном сервере:
#### Подключитесь к вашему удалённому серверу:

```
ssh -i путь_до_файла_с_SSH_ключом/название_файла_с_SSH_ключом имя_пользователя@ip_адрес_сервера
```
#### Установите Docker Compose:

```
sudo apt update
sudo apt install curl
curl -fSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh
sudo apt-get install docker-compose-plugin
```

#### Создайте на сервере директорию kittygram через терминал:

```
mkdir kittygram
```
#### В директорию kittygram/ скопируйте файлы docker-compose.production.yml и .env:

```
scp -i path_to_SSH/SSH_name docker-compose.production.yml username@server_ip:/home/username/kittygram/docker-compose.production.yml
```
#### Для запуска Docker Compose в режиме демона команду docker compose up нужно запустить с флагом -d. Выполните эту команду на сервере в папке kittygram/:
```
sudo docker compose -f docker-compose.production.yml up -d
```
Проверьте, что все нужные контейнеры запущены:
```
sudo docker compose -f docker-compose.production.yml ps
```
#### Выполните миграции, соберите статические файлы бэкенда и скопируйте их в /backend_static/static/:
```
sudo docker compose -f docker-compose.production.yml exec backend python manage.py migrate
sudo docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
sudo docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. /backend_static/static/
```
#### На сервере в редакторе nano откройте конфиг Nginx:

```
sudo nano /etc/nginx/sites-enabled/default
```

#### Измените настройки location в секции server:

```
    location / {
        proxy_set_header Host $http_host;
        proxy_pass http://127.0.0.1:9000;
    }
```

#### Проверьте работоспособность конфигураций и перезапустите Nginx:

```
sudo nginx -t 
sudo service nginx reload
```
## Технологии:

- Python  
- Django  
- React  
- Node.js 
- Nginx  
- Gunicorn  
- Certbot 
- Docker
- Postgres
- Git 

### Автор: 
[Даниил Варлащенко](https://github.com/ViaDo1orosa)
