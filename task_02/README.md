# Ansible Playbook: ClickHouse & Vector Installation

Автоматизированная установка и настройка СУБД ClickHouse и агента сбора логов Vector на Ubuntu (localhost).

##  Описание

Playbook состоит из двух play:

### Play 1: ClickHouse
- Устанавливает зависимости для HTTPS-репозиториев
- Подключает официальный APT-репозиторий ClickHouse
- Устанавливает последнюю версию пакетов: `clickhouse-common-static`, `clickhouse-client`, `clickhouse-server`
- Запускает сервис через handler
- Создаёт базу данных `logs`

### Play 2: Vector
- Скачивает бинарный дистрибутив Vector (tar.gz) с GitHub
- Распаковывает архив и извлекает бинарник в `/opt/vector/bin/vector`
- Деплоит конфигурацию `/etc/vector/vector.toml` через Jinja2-шаблон
- Устанавливает systemd-юнит `/etc/systemd/system/vector.service`
- Включает автозапуск сервиса
- Перезапускает Vector при изменении конфигурации (handler)

## Параметры (переменные)

Переменные задаются в директории `group_vars/`:

### ClickHouse (`group_vars/clickhouse/vars.yml`)
| Переменная | Описание | Значение |
|------------|----------|----------|
| `clickhouse_version` | Версия ClickHouse | последняя из репозитория |

### Vector (`group_vars/vector/vars.yml`)
| Переменная | Описание | Значение по умолчанию |
|------------|----------|------------------------|
| `vector_version` | Версия Vector | `0.34.0` |
| `vector_arch` | Архитектура Vector | `x86_64-unknown-linux-gnu` |
| `vector_install_dir` | Директория установки Vector | `/opt/vector` |

# Теги

Playbook поддерживает теги для выборочного запуска:

### установка только ClickHouse
ansible-playbook -i inventory/prod.yml site.yml --tags clickhouse --ask-become-pass

### установка только Vector
ansible-playbook -i inventory/prod.yml site.yml --tags vector --ask-become-pass

### запуск всех play
ansible-playbook -i inventory/prod.yml site.yml --ask-become-pass

# Скриншоты выполнения заданий №5-8
5. 

<img width="969" height="63" alt="image" src="https://github.com/user-attachments/assets/596873ee-f419-4d4f-b93b-e8586ac5536a" />

6.

<img width="1186" height="786" alt="image" src="https://github.com/user-attachments/assets/9708ab0b-f485-4dd5-89f6-9cb2e914ce51" />

7.

<img width="1216" height="756" alt="image" src="https://github.com/user-attachments/assets/860cbe14-b501-49eb-81bb-efe48dfa123f" />

<img width="1149" height="744" alt="image" src="https://github.com/user-attachments/assets/3f0359f5-0f44-499a-9e5c-5377483d6116" />

<img width="1198" height="602" alt="image" src="https://github.com/user-attachments/assets/bae0a11d-da8e-427c-a300-fb44ec6a23ef" />

<img width="1160" height="444" alt="image" src="https://github.com/user-attachments/assets/f070a8a4-947c-4c5f-bce8-fb557911a6c8" />

8.

<img width="1194" height="492" alt="image" src="https://github.com/user-attachments/assets/acbec95c-f194-4454-9b45-c4d4356e7e3a" />
