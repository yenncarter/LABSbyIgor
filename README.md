# LABSbyIgor
Лабораторные работы по Инженерно-технической поддержке и сопровождения информационных систем (преп Давыдов А.А)

---

## **1 Лаба**
**Установка wget**
После завершения вышеописанных действий команда:

    sudo yum install wget
будет успешно выполнена, и пакет wget будет установлен на систему.

![image](https://github.com/user-attachments/assets/449797c8-6a78-47d1-9477-d26c20c162ce)

Для проверки успешной установки wget, выполните следующую команду:

    wget --version
Если всё сделано правильно, мы увидим информацию о версии программы.

![image](https://github.com/user-attachments/assets/436410b6-1511-480d-acf0-97102dc99753)

---

## **2 Лаба**
**Установка Docker**

**1. Установка curl**
Устанавляваем sudo yum install curl.
curl это инструмент для передачи данных с сервера или к серверу через URL.

![image](https://github.com/user-attachments/assets/8a1661ef-ffe4-4218-aca8-760b00dad6ab)

---

**2. Добавление репозитория Docker**

Чтобы установить актуальную версию Docker, необходимо добавить официальный репозиторий Docker в систему. Выполните следующую команду.

    sudo wget -P /etc/yum.repos.d/ https://download.docker.com/linux/centos/docker-ce.repo
sudo - используется для выполнения команды с правами суперпользователя (root)
wget - это утилита командной строки для загрузки файлов с интернета
-P /etc/yum.repos.d/ - указывает путь, куда будет сохранен загруженный файл.
https://download.docker.com/linux/centos/docker-ce.repo - URL-адрес файла репозитория Docker.

![image](https://github.com/user-attachments/assets/471aa574-9518-4200-bbad-d5cdf80f8db0)

---

**3. Установка Docker**
После добавления репозитория можно приступить к установке Docker. Выполните следующую команду и соглашаемся со всем

        sudo yum install docker-ce docker-ce-cli containerd.io

![image](https://github.com/user-attachments/assets/a2d922e4-78d9-45a6-bc17-16a35db42a92)

---

**4. Запуск службы Docker**
После установки необходимо запустить службу Docker и включить её автозапуск при загрузке системы. Выполните следующую команду.

        sudo systemctl enable docker --now
systemctl — это утилита для управления системными службами (сервисами) в Linux.
enable — добавляет службу в автозапуск, чтобы она автоматически стартовала при загрузке системы.
--now — дополнительно к enable сразу запускает службу Docker (так же как и systemctl start docker).

(скрин с запуском доккера к сожалению где-то потерялся)

---

## **3 Лаба **

**1. Получение последней версии Docker Compose**
Для получения последней версии Docker Compose используется следующая команда:

        COMVER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d\" -f4)

Описание: Команда использует API GitHub для получения информации о последнем релизе Docker Compose и извлекает номер версии из JSON-ответа.

![image](https://github.com/user-attachments/assets/f67af534-51fe-4099-bfa0-20d78d94d6c1)

---

**2. Скачивание и установка Docker Compose**
После получения версии, скачиваем и устанавливаем Docker Compose:

        sudo curl -L "https://github.com/docker/compose/releases/download/$COMVER/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose
**Описание:**
$COMVER: Версия, полученная на предыдущем шаге.
$(uname -s): Операционная система (например, Linux).
$(uname -m): Архитектура процессора (например, x86_64).
Файл сохраняется в /usr/bin/docker-compose.

---

**3. Проверка установленной версии Docker Compose**
Даем исполняемые права файлу и проверяем установленную версию:

        sudo chmod +x /usr/bin/docker-compose
        docker-compose --version
Описание: Первая команда добавляет права выполнения, а вторая проверяет версию установленного Docker Compose.

![image](https://github.com/user-attachments/assets/2db1abc7-024b-42d4-b1f4-9a0a093fd8b4)

---

**4. Клонирование репозитория с конфигурацией Grafana**
Устанавливаем Git и клонируем репозиторий:

        sudo yum install git
        git clone https://github.com/skl256/grafana_stack_for_docker.git
        cd grafana_stack_for_docker
**Описание:**
Устанавливаем Git через системный менеджер пакетов.
Клонируем репозиторий с конфигурацией Grafana.
Переходим в склонированный каталог.

![image](https://github.com/user-attachments/assets/3e2b43f7-0a14-4634-b19e-286bd614a24e)

---

**5. Подготовка директорий и файлов конфигурации**
Создаем необходимые директории и файлы:

        sudo mkdir -p /mnt/common_volume/swarm/grafana/config
        
sudo: команда выполняется с правами суперпользователя.
mkdir: создает новую директорию.
-p: если родительские директории не существуют, они также будут созданы.
/mnt/common_volume/swarm/grafana/config: путь к новой директории, которая будет создана.

    sudo mkdir -p /mnt/common_volume/grafana/{grafana-config,grafana-data,prometheus-data}

Создает три директории внутри /mnt/common_volume/grafana/:
grafana-config
grafana-data
prometheus-data
{} используется для создания нескольких директорий одним вызовом mkdir.

    sudo chown -R $(id -u):$(id -g) {/mnt/common_volume/swarm/grafana/config,/mnt/common_volume/grafana}

chown: изменяет владельца файла или директории.
-R: рекурсивно применяет изменения ко всем файлам и поддиректориям.
$(id -u): возвращает UID текущего пользователя.
$(id -g): возвращает GID группы текущего пользователя.
{/mnt/common_volume/swarm/grafana/config,/mnt/common_volume/grafana}: список директорий, для которых меняется владелец.

    touch /mnt/common_volume/grafana/grafana-config/grafana.ini

touch: создает новый пустой файл (или обновляет метку времени существующего файла).
/mnt/common_volume/grafana/grafana-config/grafana.ini: путь к новому файлу.

    cp config/* /mnt/common_volume/swarm/grafana/config/

cp: копирует файлы.
config/*: все файлы из директории config.
/mnt/common_volume/swarm/grafana/config/: целевая директория, куда будут скопированы файлы.

    mv grafana.yaml docker-compose.yaml

mv: перемещает или переименовывает файл.
grafana.yaml: исходный файл.
docker-compose.yaml: новый файл (результат переименования).

---

**6. Запуск контейнеров Docker**
Запускаем контейнеры Docker в фоновом режиме:

        sudo docker compose up -d
   
Описание: Команда запускает все контейнеры, описанные в файле docker-compose.yaml, в фоновом режиме.

![image](https://github.com/user-attachments/assets/c2d80b12-787a-4250-a9f0-fee7dec82984)
![image](https://github.com/user-attachments/assets/02fec8d5-91fd-4d87-ae1a-f1dcc866d382)

## **4 Лаба**
Запуск контейнеров Docker в фоновом режиме
-d Он означал что мы вошли в detached mode.

    vi 
    sudo docker compose up -d

![image](https://github.com/user-attachments/assets/a00a1e1d-4f95-472c-b358-26e46c297964)

---

**Остановка контейнеров Docker**

    sudo docker compose stop

Что делает: Останавливает все запущенные контейнеры, определённые в файле docker-compose.yml, но не удаляет их.
Пояснение: Контейнеры остаются в состоянии "остановленных", и их можно снова запустить командой docker compose start.
Пример: Когда нам нужно временно остановить работу приложения, но сохранить состояние контейнеров для дальнейшего использования.

![image](https://github.com/user-attachments/assets/17cf948d-7006-46e7-8af0-9099927ff9e1)

---

**Удаление контейнеров и ресурсов Docker**

    sudo docker compose down

Что делает: Полностью останавливает и удаляет все контейнеры, сети, объёмы данных (volumes) и другие ресурсы, созданные с помощью docker-compose.yml (рис. 3).
Пояснение: Это более радикальная команда, чем stop. После выполнения этой команды все данные, хранящиеся в объёмах, будут удалены, если явно не указано их сохранение.
Пример: Когда нам нужно полностью очистить окружение и начать с чистого листа.

![image](https://github.com/user-attachments/assets/49351a02-cc08-457c-8b41-c5bde11ec726)

---

**Просмотр статуса контейнеров Docker**

    sudo docker compose ps

Что делает: Показывает статус всех контейнеров, определённых в файле docker-compose.yml (рис. 4).
Пояснение: Команда выводит список контейнеров вместе с их состоянием (запущен/остановлен), портами, сетями и другими параметрами.
Пример: Используется для мониторинга текущего состояния контейнеров.

![image](https://github.com/user-attachments/assets/d28a654a-a748-43c9-967e-fececc0e7b89)

---

**Клонирование Git-репозитория**
Командой выполняем клонирование удаленного Git-репозитория с GitHub в папку.

    git clone https://github.com/sksi1217/BobrovLabs.git

![image](https://github.com/user-attachments/assets/0e9528e2-653c-488e-93d1-fd83ebe6ca52)

---

**Определение текущего рабочего каталога**

Команда выводит адрес текущего рабочего каталога.

    pwd

![image](https://github.com/user-attachments/assets/4afef864-57b0-4b08-9a31-035416b3c874)

---

**Перемещение файлов и создание резервной копии**

    mv Igor_Labs/prometeus.yaml ./
    mv Igor_Labs/docker-compose.yaml ./

Перемещаем файлы в корень grafana_stack_for_docker, но перед этим нужно сделать Бэкап придыдущего файла docker-compose.yaml.

**Бэкап** - это резервная копия ценных данных, которая хранится отдельно от этих данных и может быть использована для их восстановления.

---

## **5 ЛАБА**

Перемещение файла prometeus.yaml

Перемещаем файл prometeus.yaml в /mnt/common_volume/swarm/grafana/config/ заменяя предыдущий файл.

    sudo mv prometeus.yaml /mnt/common_volume/swarm/grafana/config/

---

**Запуск контейнеров Docker**

Поднял контейнеры Docker через команду, для того чтобы зайти в него через браузер.

    sudo docker compose up -d

---

**Настройка Grafana через браузер**
Чтобы зайти в него нужно прописать в поиске

    localhost:3000

**Пароль и логин: admin admin**

---

**Добавление источника данных Prometheus**

После того как зашли, нужно создать Dashboards. Путь где его можно создать Home -> Connections -> Data sources -> Add data source

Где нужно нажать на +Add visualization -> Configure a new data source -> Prometheus

Настройки:
Connection: **http://prometheus:9090**
Authentication: **Basic authentication**
После того как все настроили нажимаем Save & test

---

**Импорт дашборда**
Cоздав Dashboards импортируем его: Путь где его можно импортировать Home -> Dashboards -> Import dashboard

В поле нужно написать 1860 -> Load. Select Prometheus -> Import -> Название Prometheus

![image](https://github.com/user-attachments/assets/9c655a52-5809-4a8f-8485-fdd51cee9aba)

![image](https://github.com/user-attachments/assets/538f6597-0548-4561-9a13-e159e1e3af96)





















