# Конспект по Docker

- [Определения](#Определения)
- [Системные команды](#Системные-команды)
- [Команды для работы с контейнерами](#Команды-для-работы-с-контейнерами)
- [Кейсы работы с контейнерами](#Кейсы-работы-с-контейнерами)
- [Команды для работы с сетями](#Команды-для-работы-с-сетями)
- [Кейсы работы с сетями](#Кейсы-работы-с-сетями)
- [Registry](#registry)
- [Images](#Images)
- [Команды в dockerfile](#Команды-в-dockerfile)
- [Команды для работы volume](#Команды-для-работы-volume)
- [Команды для работы docker-compose](#Команды-для-работы-docker-compose)

![alt text](https://www.researchgate.net/profile/Yahya_Al-Dhuraibi/publication/308050257/figure/fig1/AS:433709594746881@1480415833510/High-level-overview-of-Docker-architecture.png)

![alt text](https://i.imgur.com/azGGyfw.png)

### Определения

- **Doсker Container** - изолированный процесс, который ограничивает ресурсы, которое содержит приложение и относящийся к нему код. Это стандартная единица программного обеспечения,которая упаковывает код и все его зависимости. Контейнеры создаются на основе docker image (образов)
- **Виртуальная машина** - получает часть ОЗУ и ЦП хост-машины, с запуском всех процессов отдельной ОС.
- **Docker Engine** — легковесная среда выполнения, которая управляет образами, контейнерами, сборками образов и т.д.
- **Docker Daemon** — демон выполняет команды которые были отправлены клиентом docker. Сборка образов, запуск контейнеров и т.д.
- **Dockerfile** — файл с набором инструкций который используется для сборки образов (docker image)
- **Docker Image** — файл состоящий из множества слоёв, который используется для выполнения кода в докер контейнерах. Read-only template.
- **Union File Systems** — своего рода объединяемая (stackable) файловая система, которая содержит файлы и каталоги разных файловых систем. Они прозрачно накладываются друг на друга, образуя единую файловую систему.
- **Docker Volumes** — часть данных контейнеров которые ссылаются на внешние носители. Персистентно сохранять данные внутри контейнеров можно только при наличии docker volumes.
- **Docker Container** — это стандартная единица программного обеспечения,которая упаковывает код и все его зависимости. Контейнеры создаются на основе docker image (образов)
- **Docker Network** — это внутренняя сеть docker которая служит для объединения контейнеров в разные подсети и изоляции контейнеров на уровне сети
- **Docker hub** — облачное хранилище предназначенное для создания публичных и приватных репозиториев для образов. Репозиторий для работы с Docker по умолчанию.
- **Docker Registry** — репозиторий для хранения докер образов.
- **Dockerfile** — файл с набором инструкций предназначенный для создания образов.
- **Bind mount** — файл или директория на диске host системы.
- **Docker volume** — механизм для постоянного сохранения данных генерируемых и используемых контейнером.
- **TMPFS** — механизм для временного сохранения данных генерируемых и используемых контейнером в оперативной памяти host системы.
- **Docker-compose** - механизм с помощью которого можно запускать пакет контейнеров

[назад](#Конспект-по-Docker)

### Системные команды

```bash
sudo systemctl start docker
sudo systemctl status docker
sudo systemctl stop docker
```

[назад](#Конспект-по-Docker)

### Команды для работы с контейнерами

- 👑 CONTAINER ID — идентификатор контейнера
- 🗒 IMAGE — образ на основании которого был создан контейнер
- 🔥 COMMAND — команда которая используется как основной процесс
- ⏱ CREATED — время когда был создан контейнер
- 📈 STATUS — статус контейнера (запущен, на паузе, остановлен с ошибкой и т.д.)
- 🎮 PORTS — внутренние порты и мапинг портов
- 👐 NAMES — имя контейнера

| команды                                                  | описание                                                                          |
| -------------------------------------------------------- | --------------------------------------------------------------------------------- |
| docker version                                           | проверка версии                                                                   |
| docker info                                              | информация о docker                                                               |
| docker images                                            | показать список образов                                                           |
| docker rmi nginx                                         | удалить образ nginx                                                               |
| docker system prune -a -f                                | удалить все неиспользуемые containers/images/networks                             |
| docker container run - -rm hello-world                   | создание и запуск и удаление контейнера после остановки                           |
| docker container create <options> <image name:tag>       | создание контейнера                                                               |
| docker container ls                                      | информация о запущенных контейнерах                                               |
| docker container ls -a                                   | информация о всех контейнерах                                                     |
| docker container run -p 80:80 nginx                      | запуск контейнера из образа nginx, открытие 80 порта и перенаправление на 80 порт |
| docker container run -p 80:80 -d nginx                   | запуск сервиса в бэкграунде                                                       |
| docker container run - -name webhost nginx               | задать имя webhost контейнеру                                                     |
| docker container rm <options> <container name> or <hash> | удаление только остановленного контейнера                                         |
| docker container rm -f <container name> or <hash>        | удаление запущенного контейнера                                                   |
| docker container stop <container name> or <hash>         | завершение всех процессов в контейнере                                            |
| docker container pause <container name> or <hash>        | заморозка всех процессов в контейнере                                             |
| docker container start <container name> or <hash>        | запуск остановленного контейнера                                                  |
| docker container unpause <container name> or <hash>      | запуск замороженного контейнера                                                   |
| docker container inspect <container name> or <hash>      | подробная информация о контейнере                                                 |
| docker container top <container name> or <hash>          | показать запущенные процессы                                                      |
| docker container stats                                   | мониторинг в реал-тайме                                                           |

[назад](#Конспект-по-Docker)

### Кейсы работы с контейнерами

```bash
# Создаём и запускаем nginx контейнер
docker container run nginx
```

```bash
# Создаём и запускаем nginx контейнер
# 1. в фоновом режиме
docker container run -d nginx

# OR
# docker container run --detached nginx
```

```bash
# Создаём и запускаем nginx контейнер
# 1. В фоновом режиме
# 2. Мапим порт к host машине
# Запуск nginx-сервера на localhost:80
docker container run -d -p 80:80 nginx

# OR
# docker container run --detached --publish 80:80 nginx

# Запуск еще один nginx-сервер на localhost:8080
docker container run -d -p 8080:80 nginx
```

```bash
# Создаём и запускаем nginx контейнер
# 1. В фоновом режиме
# 2. Мапим порт к host машине
# 3. Устанавливаем имя контейнера
docker container run -d -p 80:80 --name proxy nginx
```

```bash
# Создаём и запускаем nginx контейнер
# 1. В фоновом режиме
# 2. Мапим порт к host машине
# 3. Устанавливаем имя контейнера
docker container run -d -p 80:80 --name proxy nginx

#  Вернёт ошибку так как имя контейнера должно быть уникальным
docker container run -d -p 8080:80 --name proxy nginx
```

```bash
# Создаём и запускаем nginx контейнер
# 1. В фоновом режиме
# 2. Мапим порт к host машине
# 3. Устанавливаем имя контейнера
# 4. Определяем healthcheck
docker container run -d -p 80:80 --name proxy --health-cmd 'curl http://localhost:80/' --health-retries 3 --health-interval '1s' nginx

# Для того что бы исправить статус на healthy необходимо установить в контейнере curl.
# docker container exec -it proxy bash
# apt-get update && apt-get install curl -y
# exit
```

```bash
# Создаём и запускаем nginx контейнер
# 1. В интерактивном режиме
# 2. Мапим порт к host машине
# 3. Устанавливаем имя контейнера
docker container run -p 80:80 --name proxy -i nginx

#OR
# docker container run -d -p 80:80 --name proxy --interactive nginx
```

```bash
# Создаём и запускаем nginx контейнер
# 1. В фоновом режиме
# 2. Мапим порт к host машине
# 3. Устанавливаем имя контейнера
# 4. Устанавливаем хард лимит занимаемой памяти
docker container run -d -p 80:80 --name proxy -m 10485760 nginx

#OR
# docker container run -d -p 80:80 --name proxy --memory 10485760 nginx

#NB limit in bytes. 10485760 = 10Mb (10 * 1024 * 1024)
```

```bash
# Создаём и запускаем nginx контейнер
# 1. В фоновом режиме
# 2. Мапим порт к host машине
# 3. Устанавливаем имя контейнера
# 4. Устанавливаем хард и софт лимит занимаемой памяти
docker container run -d -p 80:80 --name proxy -m 10485760 --memory-reservation 5242880 nginx

# 10485760 = 10Mb
# 5242880 = 5Mb
# --memory-reservation = baseline
```

```bash
# Создаём и запускаем nginx контейнер
# 1. В фоновом режиме
# 2. Мапим порт к host машине
# 3. Устанавливаем имя контейнера
# 4. Устанавливаем хард лимит и лимит swap который может использовать контейнер
docker container run -d -p 80:80 --name proxy -m 5242880 --memory-swap 10485760 nginx
```

```bash
# Создаём и запускаем nginx контейнер
# 1. В фоновом режиме
# 2. Мапим порт к host машине
# 3. Устанавливаем имя контейнера
# 4. Устанавливаем политику рестарта контейнера
docker container run -d -p 80:80 --name proxy --restart always nginx

# no — default
# on-failure — перезагрузить в случае ошибки
# always — всегда перезагружать
# unless-stopped — пока контейнер не будет остановлен
```

```bash
# Создаём и запускаем nginx контейнер
# 1. В фоновом режиме
# 2. Мапим порт к host машине
# 3. Устанавливаем имя контейнера
# 4. Удаляем контейнер после его оcтановки
docker container run --rm -d -p 80:80 --name proxy nginx
```

```bash
# Создаём и запускаем nginx контейнер
# 1. В интерактивном режиме
# 2. Устанавливаем имя контейнера
# 3. Подключаем псевдо TTY
docker container -p 80:80 run -i -t --name proxy ubuntu bash

#OR
# docker container run -it --name proxy ubuntu bash
```

```bash
# Создаём nginx контейнер
# 1. Мапим порт к host машине
# 2. Устанавливаем имя контейнера
docker container create -p 80:80 --name proxy nginx # 09b5b111388f12b06ddead99ae648718fe0086acd331c28e7ccf14bdabab7ab9

# Показывает только запущенные контейнеры
docker container ps
```

```bash
# Создаём nginx контейнер
# 1. Мапим порт к host машине
# 2. Устанавливаем имя контейнера
docker container create -p 80:80 --name proxy nginx

# Показывает все контейнеры
docker container ps -a
```

```bash
# Создаём nginx контейнер
# 1. В фоновом режиме
# 2. Мапим порт к host машине
# 3. Устанавливаем имя контейнера
docker container run -d -p 80:80 --name proxy nginx

# Присоединяет stdout, stderror к контейнеру
# docker container attach proxy
```

```bash
# Создаём nginx контейнер
# 1. В фоновом режиме
# 2. Мапим порт к host машине
# 3. Устанавливаем имя контейнера
docker container run -d -p 80:80 --name proxy nginx

# Создать новый образ на основании контейнера
docker container commit --author "Name Surname name@mail.com" --message "Add curl" proxy konet24/nginx-curl:0.0.1
```

```bash
# Создаём nginx контейнер
# 1. В фоновом режиме
# 2. Мапим порт к host машине
# 3. Устанавливаем имя контейнера
docker container run -d -p 80:80 --name proxy nginx

# Получить подробную информацию о контейнере
docker container inspect proxy
```

```bash
# Создаём nginx контейнер
# 1. В фоновом режиме
# 2. Мапим порт к host машине
# 3. Устанавливаем имя контейнера
docker container run -d -p 80:80 --name proxy nginx

# Получить отформатированную информацию о контейнере
docker container inspect proxy --format "IP: {{ .NetworkSettings.IPAddress }} | Gateway: {{.NetworkSettings.Networks.bridge.Gateway}}"
```

```bash
# Создаём nginx контейнер
# 1. В фоновом режиме
# 2. Мапим порт к host машине
# 3. Устанавливаем имя контейнера
docker container run -d -p 80:80 --name proxy1 nginx
docker container run -d -p 8080:80 --name proxy2 nginx

# Получить подробную информацию о контейнере
docker container rename proxy1 proxy
docker container rename proxy2 webhost
```

```bash
# Создаём nginx контейнер
# 1. В фоновом режиме
# 2. Мапим порт к host машине
# 3. Устанавливаем имя контейнера
docker container run -d -p 80:80 --name proxy nginx

# Удалить контейнер
# Вызовет ошибку если контейнер запущен
docker container rm proxy
```

```bash
# Создаём nginx контейнер
# 1. В фоновом режиме
# 2. Мапим порт к host машине
# 3. Устанавливаем имя контейнера
docker container run -d -p 80:80 --name proxy nginx

# получить real time информацию о ресурсах контейнера
docker container stats
```

```bash
# Создаём nginx контейнер
# 1. В фоновом режиме
# 2. Мапим порт к host машине
# 3. Устанавливаем имя контейнера
for VARIABLE in 1 2 3
do
	docker container create --name proxy-$VARIABLE nginx
done

# удалить все остановленные контейнеры
docker container prune
```

```bash
# Создаём nginx контейнер
# 1. В фоновом режиме
# 2. Мапим порт к host машине
# 3. Устанавливаем имя контейнера
docker container run -d -p 80:80 --name proxy nginx

# получить информацию о портах контейнера
docker container port proxy
```

```bash
# Создаём nginx контейнер
# 1. В фоновом режиме
# 2. Мапим порт к host машине
# 3. Устанавливаем имя контейнера
docker container run -d -p 27017:27017 --name db mongo

# получить логи контейнера
docker container logs db
```

```bash
# Создаём nginx контейнер
# 1. В фоновом режиме
# 2. Мапим порт к host машине
# 3. Устанавливаем имя контейнера
docker container run -d -p 80:80 --name proxy nginx

# выполнить команду внутри контейнера
docker container exec -it proxy bash
```

```bash
# Создаём nginx контейнер
# 1. В фоновом режиме
# 2. Мапим порт к host машине
# 3. Устанавливаем имя контейнера
docker container run -d -p 80:80 --name proxy nginx

# скопировать файлы и папки внутрь контейнера
docker container cp ./data/*.html proxy:/usr/share/nginx/html
docker container cp ./data/css proxy:/usr/share/nginx/html/css
```

```bash
# Создаём nginx контейнер
# 1. В фоновом режиме
# 2. Мапим порт к host машине
# 3. Устанавливаем имя контейнера
docker container run -d -p 80:80 --name proxy nginx

# скопировать файлы и папки внутрь контейнера
docker container cp ./data/*.html proxy:/usr/share/nginx/html
docker container cp ./data/css proxy:/usr/share/nginx/html/css

# получить изменения в контейнере
docker container diff proxy

# A	файл или директория были добавлены
# D	файл или директория были удалены
# C	файл или директория были изменены
```

```bash
# Создаём nginx контейнер
# 1. В фоновом режиме
# 2. Мапим порт к host машине
# 3. Устанавливаем имя контейнера
docker container run -d -p 80:80 --name proxy nginx

# рестарт контейнера через 5с
sleep 5
docker container restart proxy
```

[назад](#Конспект-по-Docker)

### Команды для работы с сетями

Типы драйверов:

- 🌉 bridge - сеть, которая используется для изолирования контейнеров
- 🖥 host - используется для присоединения сети контейнеров к хостовой системе
- 💼 overlay - используется для построения кластеров, для соединения нескольких докер-демонов
- 🎮 macvlan - позволяет драйвер примапить к mac хостовой системы
- 🧐 none - полностью изолированная сеть
- 👍 network plugins - добавление доп. плагинов

| команды    | описание                             |
| ---------- | ------------------------------------ |
| connect    | подключить контейнер к сети          |
| create     | создать сеть                         |
| disconnect | отключить контейнер от сети          |
| inspect    | получить подробную информацию о сети |
| ls         | получить список сетей                |
| prune      | удалить неиспользуемые сети          |
| rm         | удалить определенную сеть            |

[назад](#Конспект-по-Docker)

### Кейсы работы с сетями

```bash
# Создаём сеть
docker network create frontend

# #OR
# docker network create -d bridge frontend
```

```bash
# Создаём сеть
docker network create frontend

# Получить список всех существующих сетей.
docker network ls
```

```bash
# Создаём сеть
docker network create frontend

# Запустить контейнер с nginx.
docker container run -d -p 80:80 --name proxy nginx

# Присоединить контейнер к сети
docker network connect frontend proxy
```

```bash
# Создаём сеть
docker network create frontend

# Запустить контейнер с nginx.
docker container run -d -p 80:80 --name proxy nginx

docker container inspect proxy --format "{{ .NetworkSettings.Networks }}" # one network

# Присоединить контейнер к сети
docker network connect frontend proxy

docker container inspect proxy --format "{{ .NetworkSettings.Networks }}" # two networks

# Отсоединить контейнер от сети
docker network disconnect frontend proxy

docker container inspect proxy --format "{{ .NetworkSettings.Networks }}" # one network
```

```bash
# Создаём сеть
docker network create frontend

# Запустить контейнер с nginx.
# docker container run -d -p 80:80 --name proxy nginx

#OR
# добавляет контейнер в определённую сеть
docker container run -d -p 80:80 --name proxy --net frontend nginx

# Получить детальную информацию о сети
docker network inspect frontend
```

```bash
# Создаём сеть
docker network create frontend

# Запустить контейнер с nginx.
docker container run -d -p 80:80 --name proxy nginx

docker container run -d -p 80:80 --name proxy --net frontend nginx

# Удалить все неиспользуемые сети
docker network prune
```

```bash
# Создаём сеть
docker network create frontend

# Запустить контейнер с nginx c dns-именем search
docker container run -d --net frontend --net-alias search elasticsearch:2
docker container run -d --net frontend --net-alias search elasticsearch:2

# Определяем ip по dns-имени
docker container run --rm --net frontend alpine nslookup search

# curl server info
docker container run --rm --net frontend centos curl -s search:9200
```

[назад](#Конспект-по-Docker)

### Registry

```bash
# Создать локальный registry
docker container run -d -p 5000:5000 --restart always --name registry registry:2
```

```bash
# Запушить образ ubuntu в registry
docker push localhost:5000/ubuntu
```

```bash
# Скачиваем образ на локальный компьютер
docker pull nginx

# Задаём новое имя образу
docker image tag nginx konet24/nginx

# Пушим образ в пользовательский репозиторий
docker push konet24/nginx
```

```bash
# запускаем локальный registry
# 1. Скачиваем образ и запускаем контейнер registry 2-й версии
# 2. Запускаем в фоновом режиме
# 3. Мапим 5000 порт локальной машины на 5000 порт контейнера
# 4. Настраиваем политику рестарта контейнера на — always
# 5. Задаём имя контейнеру registry
docker run -d -p 5000:5000 --restart always --name registry registry:2
```

```bash
# Скачиваем образ на локальный компьютер
docker pull nginx

# Задаём новое имя образу (локальный registry)
docker image tag nginx localhost:5000/nginx

# Пушим образ в пользовательский репозиторий
docker push localhost:5000/nginx

# Путь по которому сохраняются образы в локальном репозитории
# /var/lib/registry/docker/registry/v2
```

[назад](#Конспект-по-Docker)

### Images

IMAGE INFO:

- REPOSITORY
- TAG
- IMAGE ID
- CREATED
- SIZE

| команды                                                   | описание                        |
| --------------------------------------------------------- | ------------------------------- |
| docker image ls <options> <image name:tag>                | посмотреть все локальные образы |
| docker image pull <options> <image name:tag>              | скачать образ                   |
| docker image push <options> <image name:tag>              | запушить образ                  |
| docker image inspect <options> <image name:tag>           | подробная информация про образ  |
| docker image history <options> <image name:tag>           | история образа                  |
| docker image tag <source image name:tag> <image name:tag> | версия образа                   |

```bash
# Скачиваем образы для демо
docker image pull nginx
docker image pull alpine

# Получить список локально скачанных образов
docker image ls

#OR
docker images
```

```bash
# Скачиваем образы для демо
docker image pull nginx
docker image pull alpine

# Получить список локально скачанных образов
docker image ls --digests
```

```bash
# Скачиваем образы для демо
docker image pull nginx
docker image pull alpine:3.7
docker image pull alpine:2.6

# Получить список локально скачанных образов
docker image ls alpine

#OR
docker image ls alpine:3.7
```

```bash
# Скачиваем последнюю версию образа на локальный компьютер
docker pull nginx

# Аналогично команде выше
docker pull nginx:latest
```

```bash
# Скачиваем определённую версию образа на локальный компьютер
docker pull nginx:1.13
```

```bash
# Скачиваем образ на локальный компьютер
docker pull --all-tags alpine

#OR
docker pull -a alpine
```

```bash
# Скачиваем образ на локальный компьютер
docker pull alpine

# Скачает все доступные образы alpine
docker pull -a alpine
```

```bash
# Скачиваем образ на локальный компьютер
docker pull alpine:latest
# Скачает тот же образ что и latest так как 3.9 поседняя текущая верси
docker pull alpine:3.9
# Скачает тот же образ что и latest так как 3.9.3 поседняя текущая верси
docker pull alpine:3.9.3
```

```bash
# Скачиваем образ на локальный компьютер
docker pull nginx:latest

# Скачает ту же версию nginx но с другим базовым образом
docker pull nginx:1.15.12-alpine
```

```bash
# Скачиваем пользовательский образ на локальный компьютер
docker image pull fholzer/nginx-brotli
```

```bash
# Скачиваем пользовательский образ на локальный компьютер
docker image pull fholzer/nginx-brotli

# Получаем подробную информацию про образ
docker image inspect fholzer/nginx-brotli
```

```bash
# Скачиваем пользовательский образ на локальный компьютер
docker image pull fholzer/nginx-brotli

# Получаем подробную информацию про образ
docker image inspect fholzer/nginx-brotli --format "{{.Config.Env}}"
```

```bash
# Скачиваем пользовательский образ на локальный компьютер
docker image pull fholzer/nginx-brotli

# Получаем информацию про слои
docker image history fholzer/nginx-brotli
```

```bash
# Скачиваем пользовательский образ на локальный компьютер
docker image pull fholzer/nginx-brotli

# Получаем подробную информацию про слои
docker image history --no-trunc fholzer/nginx-brotli
```

```bash
# Скачиваем пользовательский образ на локальный компьютер
docker image pull fholzer/nginx-brotli

# Cоздаём копию образа для docker hub registry
docker image tag fholzer/nginx-brotli konet24/brotli
```

```bash
# Скачиваем пользовательский образ на локальный компьютер
docker image pull fholzer/nginx-brotli

# Cоздаём копию образа для docker hub registry
docker image tag fholzer/nginx-brotli konet24/brotli
docker image tag fholzer/nginx-brotli konet24/brotli:0.0.1
```

```bash
# Скачиваем пользовательский образ на локальный компьютер
docker image pull fholzer/nginx-brotli

# Cоздаём копию образа для third party registry
docker image tag fholzer/nginx-brotli localhost:5000/brotli

# OR
docker image tag fholzer/nginx-brotli konet24.xyz/brotli
```

```bash
# Скачиваем пользовательский образ на локальный компьютер
docker image pull fholzer/nginx-brotli

# Cоздаём копию образа для docker hub registry
docker image tag fholzer/nginx-brotli konet24/brotli
docker image tag fholzer/nginx-brotli konet24/brotli:0.0.1

# Пуш образа в репозиторий
docker image push konet24/brotli
# AND
docker image push konet24/brotli:0.0.1
```

[назад](#Конспект-по-Docker)

### Команды в dockerfile

| команды                                      | описание                                                |
| -------------------------------------------- | ------------------------------------------------------- |
| docker build . -t localhost:5000/nginx:0.0.1 | создать образ на основании dockerfile                   |
| FROM                                         | определение базового образа                             |
| RUN                                          | запуск команды на этапе build                           |
| CMD                                          | запуск команды при запуске образа                       |
| LABEL                                        | добавление мета-информации в образ                      |
| EXPOSE                                       | открытие внутренних портов контейнера                   |
| ENV                                          | определение переменные среды окружения                  |
| ADD                                          | добавить информацию в контейнер                         |
| COPY                                         | скопировать информацию в контейнер                      |
| ENTRYPOINT                                   | указать команду при запуске образа (без преопределения) |
| VOLUME                                       | сохранить данные генерируемые контейнером               |
| USER                                         | указать пользователя в контейнере                       |
| WORKDIR                                      | указать рабочий каталог внутри image                    |
| ARG                                          | указать аргументы при создании образа                   |
| ONBUILD                                      | создать цепочку независимых build-ов                    |
| STOPSIGNAL                                   | указать команду для остановки контейнера                |
| HEALTHCHECK                                  | указать команду для проверки контейнера                 |
| SHELL                                        | выполнить shell-скрипт при создании образа              |

```bash
# Базовый образ с которого мы начнём создание своего собственного образа
FROM nginx:1.16.0
```

```bash
# Задаём переменные среды окружения
ENV MODE production
ENV TEST false
```

```bash
# вызовет ошибку так как будет требовать
# подтверждения выполнения команды
RUN apt-get update \
    && apt-get install curl
```

```bash
# не вызовет в ошибку
# из-за наличия флага -y который автоматически
# подтвердит действие
RUN apt-get update \
    && apt-get install curl -y
```

```bash
# ещё одна команда котора создаёт символическу ссылку
# которая фактически будет выводить log nginx в консоль
RUN ln -sf /dev/stdout /var/log/nginx/access.log

###########################################################

RUN apt-get update \
    && apt-get install curl -y \
    && ln -sf /dev/stdout /var/log/nginx/access.log
```

```bash
# Открываем порт контейнера для внутренней Docker виртуальной сети
EXPOSE 80 443 8080 8443
```

```bash
# Команда которая будет запущена при старте нашего контейнера
CMD ["nginx", "-g", "daemon off;"]

# для приложения написанного на Node.js
CMD ["node", "index.js"]
```

```bash
# Базовый образ с которого мы начнём создание своего собственного образа
FROM nginx:1.16.0

WORKDIR /usr/share/nginx/html
#OR → но это плохая практика
# RUN cd /usr/share/nginx/html

COPY index.html index.html
#  если индексный файл находится в папке build
# COPY ./build/index.html index.html

# эти команды определять не нужно
# они присутствуют в базовом образе → FROM
# EXPOSE
# CMD
```

```bash
# Базовый образ с которого мы начнём создание своего собственного образа
FROM nginx:1.16.0

WORKDIR /usr/share/nginx/html

COPY index.html index.html
```

```bash
# escape=`
# Специальная директива которая опередялет что будет служить
# в качестве символа переноса строки

# Базовый образ с которого мы начнём создание своего собственного образа
FROM nginx:1.16.0

WORKDIR /usr/share/nginx/html

# вызовет ошибку из-за директивы escape
# RUN apt-get update \
#     && apt-get install curl -y

RUN apt-get update `
    && apt-get install curl -y

COPY index.html index.html
```

```bash
# Базовый образ с которого мы начнём создание своего собственного образа
FROM nginx:1.16.0

ENV WORKDIR /usr/share/nginx/html

WORKDIR ${WORKDIR}
# То же что и
# WORKDIR /usr/share/nginx/html

LABEL path ${WORKDIR}

# ADD
# COPY
# ENV
# EXPOSE
# FROM
# LABEL
# STOPSIGNAL
# USER
# VOLUME
# WORKDIR

COPY index.html index.html
```

```bash
# Базовый образ с которого мы начнём создание своего собственного образа
FROM nginx:1.16.0

ENV WORKDIR /usr/share/nginx/html

WORKDIR ${WORKDIR}

COPY index.html index.html

# Скопировать все
COPY . .
```

```bash
# Базовый образ с которого мы начнём создание своего собственного образа
FROM nginx:1.16.0

ENV WORKDIR /usr/share/nginx/html

# Домашний каталог
WORKDIR ${WORKDIR}

# Добавить мета информацию к образу
LABEL version="1.0" \
      maintainer="Name"

COPY index.html index.html
```

[назад](#Конспект-по-Docker)

### Команды для работы volume

| команды                | описание                         |
| ---------------------- | -------------------------------- |
| docker volume create   | создать volume                   |
| docker volume inspect  | посмотреть доп.информацию volume |
| docker volume ls       | вывести на экран volumes         |
| docker volume prune -f | удалить volumes                  |
| docker volume rm       | удалить volume                   |

```bash
# Скачиваем образ с MongoDB
docker image pull mongo

# Смотрим информацию о используемых Volume внутри контейнера
docker image inspect mongo

# OR
docker image inspect mongo --format "{{.Config.Volumes}}"
```

```bash
# Создаём Volume
docker volume create storage
docker volume create db

# Смотрим информацию о используемых Volume внутри контейнера
docker container run -d -v storage:/data/db mongo
```

```bash
# Создаём Volume
docker volume create storage
docker volume create config

# Смотрим информацию о используемых Volume внутри контейнера
docker container run -d -v storage:/data/db -v config:/data/configdb -p 27017:27017 mongo
```

```bash
# Создаём Volume
docker volume create storage
docker volume create config

# Смотрим информацию о используемых Volume внутри контейнера
docker container run -d -v storage:/data/db -v config:/data/configdb -p 27017:27017 mongo
```

```bash
# Создаём Volume
docker volume create storage
docker volume create config

# Смотрим информацию о используемых Volume внутри контейнера
docker container run -d -v storage:/data/db -v config:/data/configdb -p 27017:27017 mongo

# Контейнер не будет запущен так как Volume уже используется
docker container run -d -v storage:/data/db -v config:/data/configdb -p 27018:27017 mongo
```

```bash
# Создаём Volume
docker volume create storage
docker volume create config

# Смотрим информацию о используемых Volume внутри контейнера
docker container run -d -v storage:/data/db:ro -v config:/data/configdb -p 27017:27017 mongo

# ro - read only - контейнер не может только читать из volume
```

```bash
# Создаём Volume
docker volume create storage

# Смотрим информацию о используемых Volume внутри контейнера
docker container run -d -v storage:/usr/share/nginx/html:ro -p 80:80 nginx:alpine
```

```bash
# Создаём Volume
docker volume create --name storage --label maintainer='Name' --label used_for='mongo db storage'
```

```bash
# Создаём Volume
docker volume create --driver local \
    --opt type=tmpfs \
    --opt device=tmpfs \
    --opt o=size=400m \
    storage

docker container run -d -v storage:/data/db --name mongo mongo

# OR
docker container run -d --volume storage:/data/db --name mongo mongo
```

```bash
# Команда  mount,то же что и комада volume
docker container run -d --mount source=storage,target=/usr/share/nginx/html --name webhost nginx:alpine
```

```bash
# Type volume - по умолчанию
docker container run -d --mount type=volume,source=storage,target=/usr/share/nginx/html --name webhost nginx:alpine
```

```bash
# Type bind - присоединение внешней деректории внутрь контейнера
# Мап папки html
docker container run -d \
    --mount type=bind,source="$(pwd)"/html,target=/usr/share/nginx/html \
    --publish 80:80 \
    --name webhost nginx:alpine
```

```bash
# Мап 2 файлов
docker container run -d \
    --mount type=bind,source="$(pwd)"/index.html,target=/usr/share/nginx/html/index.html \
    --mount type=bind,source="$(pwd)"/about.html,target=/usr/share/nginx/html/about.html \
    --publish 80:80 \
    --name webhost nginx:alpine

# For windows
docker container run -d \
    --mount type=bind,source=/"$(pwd)"/index.html,target=/usr/share/nginx/html/index.html \
    --mount type=bind,source=/"$(pwd)"/about.html,target=/usr/share/nginx/html/about.html \
    --publish 80:80 \
    --name webhost nginx:alpine
```

```bash
# При моунте нужна директория, иначе ошибка
docker run -d \
  -it \
  --name webhost \
  --mount type=bind,source="$(pwd)"/target,target=/app \
  nginx:alpine

#OR
# При моунте создаёт директорию, если ее нет
docker run -d \
  -it \
  --name webhost \
  --volume "$(pwd)"/target:/app \
  nginx:alpine

# При моунте несуществующего файла, всегда создаёт директорию
docker run -d \
  -it \
  --name webhost \
  --volume "$(pwd)"/target/index.html:/app/index.html \
  nginx:alpine
```

```bash
# Ошибка, так как пустая папка target при моунте перезатрет папку target в контейнере
docker run -d \
  -it \
  --name webhost \
  --mount type=bind,source="$(pwd)"/target,target=/usr \
  nginx:alpine

#OR
docker run -d \
  -it \
  --name webhost \
  -v "$(pwd)"/target:/usr \
  nginx:alpine
```

```bash
# Пример примонтируемой директории только для чтения
docker run -d \
  -it \
  --name webhost \
  --mount type=bind,source="$(pwd)"/target,target=/app,readonly \
  nginx:alpine

#OR
docker run -d \
  -it \
  --name webhost \
  -v "$(pwd)"/target:/app:ro \
  nginx:alpine
```

```bash
# Type -tmpfs - создание volume в оперативной памяти
# Вместо target используется destination
docker run -d \
  -it \
  --name webhost \
  --mount type=tmpfs,destination=/app \
  nginx:alpine

#OR
docker run -d \
  -it \
  --name webhost \
  --tmpfs /app \
  nginx:alpine
```

```bash
# Ограничение каталога volume
docker run -d \
  -it \
  --name webhost \
  --mount type=tmpfs,destination=/app,tmpfs-size=100m \
  nginx:alpine
```

```bash
# При создании образа на основании Dockerfile volume создается автоматически

# Содержимое Dockerfile
FROM nginx:alpine

WORKDIR /usr/share/nginx/html

VOLUME /usr/share/nginx/html

COPY index.html index.html

# Создаём образ на основании Dockerfile
docker build . -t konet24/landing
```

[назад](#Конспект-по-Docker)

### Команды для работы docker-compose

| команды                     | описание                                  |
| --------------------------- | ----------------------------------------- |
| docker-compose up -d        | поднять сервисы в бэграунде               |
| docker-compose down         | остановить сервисы                        |
| docker-compose logs webhost | посмотреть логи сервиса webhost           |
| docker-compose build        | собрать несколько сервисов из dockerfiles |

```bash
# Файл docker-compose должен начинаться с тега версии.
# Версии начинающие с 2 предназначены для работы вне кластера
version: "2.4"

# Следует учитывать, что docker-composes работает с сервисами.
# 1 сервис = 1 контейнер.
# Сервисом может быть клиент, сервер, сервер баз данных...
# Раздел, в котором будут описаны сервисы, начинается с 'services'.
services:
  webhost:
    image: nginx:alpine
```

```bash
# Мапинг портов
version: "2.4"

services:
  webhost:
    image: nginx:alpine
    ports:
      - 80:80
```

```bash
# Добавили файл html из хоста /usr/share/nginx/html в сервис в /html
version: "2.4"

services:
  webhost:
    image: nginx:alpine
    ports:
      - 80:80
    volumes:
      - ./html:/usr/share/nginx/html
```

```bash
# Добавили анонимный volume
version: "2.4"

services:
  webhost:
    image: nginx:alpine
    ports:
      - 80:80
    volumes:
      - /usr/share/nginx/html
```

```bash
# Задаем имя контейнера
version: "2.4"

services:
  webhost:
    image: nginx:alpine
    container_name: webhost
    ports:
      - 80:80
    volumes:
      - ./html:/usr/share/nginx/html
```

```bash
# Создаем 2 сервиса webhost и db
# Через свойство depends_on определяем очередность запуска контейнеров
# Сервис webhost не стартанет до тех пор пока не запуститься сервис db
version: "2.4"

services:
  webhost:
    image: nginx:alpine
    container_name: webhost
    ports:
      - 80:80
    depends_on:
      - db
    volumes:
      - ./html:/usr/share/nginx/html

  db:
    image: mongo:4
    container_name: storage
    volumes:
      - ./data:/data/db
```

```bash
# Добавляем перезапуск контейнера
version: "2.4"

services:
  webhost:
    image: nginx:alpine
    container_name: webhost
    ports:
      - 80:80
    restart: always
    volumes:
      - ./html:/usr/share/nginx/html
```

```bash
# Добавляем переменные среды окружения
version: "2.4"

services:
  webhost:
    image: nginx:alpine
    container_name: webhost
    ports:
      - 80:80
    restart: always
    environment:
      - MODE=dev
    volumes:
      - ./html:/usr/share/nginx/html
```

```bash
# Настройка запуска команд внутри контейнера
version: "2.4"

services:
  db:
    image: mongo:4
    container_name: storage
    command: mongod --auth
    ports:
      - 27017:27017
    volumes:
      - ./data:/data/db

# CMD ['mongod']
```

```bash
# Подключение сервисов к определенным сетям
version: "2.4"

services:
  webhost:
    image: nginx:alpine
    container_name: webhost
    ports:
      - 80:80
    depends_on:
      - db
    volumes:
      - ./html:/usr/share/nginx/html
    networks:
      - back
      # - db

  db:
    image: mongo:4
    container_name: storage
    volumes:
      - ./data:/data/db
    networks:
      - db

networks:
  back:
    driver: bridge
  db:
    driver: bridge
```

```bash
#
version: "2.4"

services:
  webapp:
    image: nginx:alpine
    container_name: webapp
    build: ./webapp
    ports:
      - 80:80
    volumes:
      - ./html:/usr/share/nginx/html
    networks:
      - back

  webhost:
    image: nginx:alpine
    container_name: webhost
    build: ./webhost
    ports:
      - 8080:80
    volumes:
      - ./html:/usr/share/nginx/html
    networks:
      - back

networks:
  back:
    driver: bridge
```

```bash
# Указываем когда можно стартовать зависимому сервису (при старте/ когда есть healthchecks)
version: "2.4"

services:
  webhost:
    image: nginx:alpine
    container_name: webhost
    ports:
      - 80:80
    depends_on:
      db:
        condition: service_started
        #condition: service_healthy
    volumes:
      - ./html:/usr/share/nginx/html
  db:
    image: mongo:4
    container_name: storage
```

```bash
# Указываем healthchecks
version: "2.4"

services:
  webhost:
    image: nginx:alpine
    container_name: webhost
    ports:
      - 80:80
    healthcheck:
      test: "curl http://localhost/"
      interval: 10s
      timeout: 5s
      retries: 3
    volumes:
      - ./html:/usr/share/nginx/html
```

[назад](#Конспект-по-Docker)
