# vector-role

Ansible-роль для установки и базовой настройки [Vector](https://vector.dev/) на Linux-сервере.

## Что делает роль

- скачивает архив Vector заданной версии для указанной архитектуры;
- распаковывает его в каталог установки и создаёт ссылку на исполняемый файл `/usr/local/bin/vector`;
- создаёт каталоги конфигурации и данных (`/var/lib/vector`);
- генерирует конфигурацию Vector и systemd unit из шаблонов;
- при изменении конфигурации или unit-файла перезапускает и включает службу `vector`.

Базовая конфигурация читает системные журналы через `journald` и выводит события в консоль в формате JSON.

## Переменные

| Переменная | По умолчанию | Назначение |
| --- | --- | --- |
| `vector_version` | `0.34.0` | Версия Vector для загрузки. |
| `vector_arch` | `x86_64` | Архитектура в имени загружаемого архива. |
| `vector_install_dir` | `/opt/vector` | Каталог распаковки Vector. |
| `vector_config_dir` | `/etc/vector` | Каталог конфигурации; основной файл — `vector.toml`. |

Значения по умолчанию определены в `defaults/main.yml`.

## Файлы роли

- `tasks/main.yml` — загрузка, установка и развёртывание конфигурации;
- `handlers/main.yml` — перезапуск systemd-службы Vector;
- `templates/vector.toml.j2` — шаблон конфигурации источника журналов и консольного sink;
- `templates/vector.service.j2` — шаблон systemd unit;
- `meta/main.yml` — метаданные роли и зависимости.

## Использование

Для использования нужно добавить роль в playbook, например:

```yaml
- hosts: vector_hosts
  become: true
  roles:
    - vector-role
```

Переменные роли можно переопределить в playbook или inventory:

```yaml
vector_version: "0.34.0"
vector_arch: "x86_64"
vector_install_dir: "/opt/vector"
vector_config_dir: "/etc/vector"
```
