# Домашнее задание к занятию 5 «Тестирование roles» - `Некрасов Александр`

---

## Подготовка к выполнению

1. Установите molecule и его драйвера: `pip3 install "molecule molecule_docker molecule_podman`.
2. Выполните `docker pull aragast/netology:latest` —  это образ с podman, tox и несколькими пайтонами (3.7 и 3.9) внутри.

## Основная часть

Ваша цель — настроить тестирование ваших ролей. 

Задача — сделать сценарии тестирования для vector. 

Ожидаемый результат — все сценарии успешно проходят тестирование ролей.

### Molecule

1. Запустите  `molecule test -s ubuntu_xenial` (или с любым другим сценарием, не имеет значения) внутри корневой директории clickhouse-role, посмотрите на вывод команды. Данная команда может отработать с ошибками или не отработать вовсе, это нормально. Наша цель - посмотреть как другие в реальном мире используют молекулу И из чего может состоять сценарий тестирования.
2. Перейдите в каталог с ролью vector-role и создайте сценарий тестирования по умолчанию при помощи `molecule init scenario --driver-name docker`.
3. Добавьте несколько разных дистрибутивов (oraclelinux:8, ubuntu:latest) для инстансов и протестируйте роль, исправьте найденные ошибки, если они есть.
4. Добавьте несколько assert в verify.yml-файл для  проверки работоспособности vector-role (проверка, что конфиг валидный, проверка успешности запуска и др.). 
5. Запустите тестирование роли повторно и проверьте, что оно прошло успешно.
5. Добавьте новый тег на коммит с рабочим сценарием в соответствии с семантическим версионированием.

### Tox

1. Добавьте в директорию с vector-role файлы из [директории](./example).
2. Запустите `docker run --privileged=True -v <path_to_repo>:/opt/vector-role -w /opt/vector-role -it aragast/netology:latest /bin/bash`, где path_to_repo — путь до корня репозитория с vector-role на вашей файловой системе.
3. Внутри контейнера выполните команду `tox`, посмотрите на вывод.
5. Создайте облегчённый сценарий для `molecule` с драйвером `molecule_podman`. Проверьте его на исполнимость.
6. Пропишите правильную команду в `tox.ini`, чтобы запускался облегчённый сценарий.
8. Запустите команду `tox`. Убедитесь, что всё отработало успешно.
9. Добавьте новый тег на коммит с рабочим сценарием в соответствии с семантическим версионированием.

После выполнения у вас должно получится два сценария molecule и один tox.ini файл в репозитории. Не забудьте указать в ответе теги решений Tox и Molecule заданий. В качестве решения пришлите ссылку на  ваш репозиторий и скриншоты этапов выполнения задания. 

### Необязательная часть

1. Проделайте схожие манипуляции для создания роли LightHouse.
2. Создайте сценарий внутри любой из своих ролей, который умеет поднимать весь стек при помощи всех ролей.
3. Убедитесь в работоспособности своего стека. Создайте отдельный verify.yml, который будет проверять работоспособность интеграции всех инструментов между ними.
4. Выложите свои roles в репозитории.

В качестве решения пришлите ссылки и скриншоты этапов выполнения задания.

---

### Как оформить решение задания

Выполненное домашнее задание пришлите в виде ссылки на .md-файл в вашем репозитории.

---

### Решение 1

Подготовка

```
python3 -m venv molecule-venv

source molecule-venv/bin/activate

pip install "molecule" "molecule-plugins[docker,podman]"

molecule --version

docker pull aragast/netology:latest
```

![Screenshot_1](img/Screenshot_1.png)


Запуск molecule test -s ubuntu_xenial
![Screenshot_2](img/Screenshot_2.png) 

Cоздание сценария тестирования по умолчанию. ```molecule init scenario```
![Screenshot_3](img/Screenshot_3.png)

В задании используется запись ```molecule init scenario --driver-name docker```, но начиная с версии >=6 синтаксис изменен.
![Screenshot_4](img/Screenshot_4.png)

Успешные прохождения тестирования ролей.
![Screenshot_5](img/Screenshot_5.png)

Внутри контейнера
```
docker run --privileged=True \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v $(pwd):/opt/vector-role \
  -w /opt/vector-role \
  -it aragast/netology:latest /bin/bash

# Команда tox
```
  
![Screenshot_6](img/Screenshot_6.png)

### Molecule. Сценарий по умолчанию (Docker)

- Создан `molecule/default/`
- Платформы: `oraclelinux-9`, `ubuntu-latest`
- Написан `verify.yml` с 5 assert-проверками
- `molecule test -s default` → **успешно** (`failed=0`)
- **Тег:** `2.0.0`

### Облегчённый сценарий (Podman)

- Создан `molecule/podman/`
- Драйвер: `podman`
- Предназначен для запуска на хосте с Podman
- **Тег:** `2.1.0`

### Tox

- Создан `tox.ini`
- Запускает `molecule test -s default` в контейнере `aragast/netology:latest`
- Автоматически устанавливает `docker-ce-cli`
- `tox` → **`congratulations :)`**
- **Тег:** `2.1.0`

### Ссылки

- Репозиторий: https://github.com/AlexandrNekrasov/vector-role
- Тег Molecule: https://github.com/AlexandrNekrasov/vector-role/releases/tag/2.0.0
- Тег Tox: https://github.com/AlexandrNekrasov/vector-role/releases/tag/2.1.0