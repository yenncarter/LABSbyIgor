# LABSbyIgor
Лабораторные работы по Инженерно-технической поддержке и сопровождения информационных систем (преп Давыдов А.А)

1 Лаба
Установка wget
После завершения вышеописанных действий команда:

sudo yum install wget
будет успешно выполнена, и пакет wget будет установлен на систему.

![image](https://github.com/user-attachments/assets/449797c8-6a78-47d1-9477-d26c20c162ce)

Для проверки успешной установки wget, выполните следующую команду:

wget --version
Если всё сделано правильно, мы увидим информацию о версии программы.

![image](https://github.com/user-attachments/assets/436410b6-1511-480d-acf0-97102dc99753)



2 Лаба
Установка Docker

1. Установка curl
Устанавляваем sudo yum install curl.
curl это инструмент для передачи данных с сервера или к серверу через URL.

![image](https://github.com/user-attachments/assets/8a1661ef-ffe4-4218-aca8-760b00dad6ab)

2. Добавление репозитория Docker

Чтобы установить актуальную версию Docker, необходимо добавить официальный репозиторий Docker в систему. Выполните следующую команду.

sudo wget -P /etc/yum.repos.d/ https://download.docker.com/linux/centos/docker-ce.repo
sudo - используется для выполнения команды с правами суперпользователя (root)
wget - это утилита командной строки для загрузки файлов с интернета
-P /etc/yum.repos.d/ - указывает путь, куда будет сохранен загруженный файл.
https://download.docker.com/linux/centos/docker-ce.repo - URL-адрес файла репозитория Docker.

![image](https://github.com/user-attachments/assets/471aa574-9518-4200-bbad-d5cdf80f8db0)

3. Установка Docker
После добавления репозитория можно приступить к установке Docker. Выполните следующую команду и соглашаемся со всем y (рис. 3)

sudo yum install docker-ce docker-ce-cli containerd.io

![image](https://github.com/user-attachments/assets/a2d922e4-78d9-45a6-bc17-16a35db42a92)

4. Запуск службы Docker
После установки необходимо запустить службу Docker и включить её автозапуск при загрузке системы. Выполните следующую команду (рис. 4).

sudo systemctl enable docker --now
systemctl — это утилита для управления системными службами (сервисами) в Linux.
enable — добавляет службу в автозапуск, чтобы она автоматически стартовала при загрузке системы.
--now — дополнительно к enable сразу запускает службу Docker (так же как и systemctl start docker).

(скрин с запуском доккера к сожалению где-то потерялся)


3 Лаба 

1. Получение последней версии Docker Compose
Для получения последней версии Docker Compose используется следующая команда:

COMVER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d\" -f4)

Описание: Команда использует API GitHub для получения информации о последнем релизе Docker Compose и извлекает номер версии из JSON-ответа.

![image](https://github.com/user-attachments/assets/f67af534-51fe-4099-bfa0-20d78d94d6c1)

2. Скачивание и установка Docker Compose
После получения версии, скачиваем и устанавливаем Docker Compose:

sudo curl -L "https://github.com/docker/compose/releases/download/$COMVER/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose
Описание:
$COMVER: Версия, полученная на предыдущем шаге.
$(uname -s): Операционная система (например, Linux).
$(uname -m): Архитектура процессора (например, x86_64).
Файл сохраняется в /usr/bin/docker-compose.

3. Проверка установленной версии Docker Compose
Даем исполняемые права файлу и проверяем установленную версию:

sudo chmod +x /usr/bin/docker-compose
docker-compose --version
Описание: Первая команда добавляет права выполнения, а вторая проверяет версию установленного Docker Compose.

![image](https://github.com/user-attachments/assets/2db1abc7-024b-42d4-b1f4-9a0a093fd8b4)


4. Клонирование репозитория с конфигурацией Grafana
Устанавливаем Git и клонируем репозиторий:

sudo yum install git
git clone https://github.com/skl256/grafana_stack_for_docker.git
cd grafana_stack_for_docker
Описание:
Устанавливаем Git через системный менеджер пакетов.
Клонируем репозиторий с конфигурацией Grafana.
Переходим в склонированный каталог.

![image](https://github.com/user-attachments/assets/3e2b43f7-0a14-4634-b19e-286bd614a24e)

5. Подготовка директорий и файлов конфигурации
Создаем необходимые директории и файлы (рис. 4):

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

6. Запуск контейнеров Docker
Запускаем контейнеры Docker в фоновом режиме:

sudo docker compose up -d
Описание: Команда запускает все контейнеры, описанные в файле docker-compose.yaml, в фоновом режиме.

![image](https://github.com/user-attachments/assets/c2d80b12-787a-4250-a9f0-fee7dec82984)
![image](https://github.com/user-attachments/assets/02fec8d5-91fd-4d87-ae1a-f1dcc866d382)














