Создал VM на Yandex Cloud, инстанс на ubuntu, пользователь dima.

```bash
ansible all -i '51.250.87.170,' -u dima -m ping

51.250.87.170 | UNREACHABLE! => {
    "changed": false,
    "msg": "Failed to connect to the host via ssh: dima@51.250.87.170: Permission denied (publickey).",
    "unreachable": true
}
```

Я не использую одну пару ssh ключей для всех сервисов, под каждый создаю новый. Поэтому либо добавляю правило в файл ~/.ssh/config для регулярно используемого соединения, либо явно указываю его в нужной команде, для ansible в режиме ad-hoc есть ключ --privat-key:

```bash
ansible all -i '51.250.87.170,' -u dima --private-key ~/.ssh/yacloud -m ping

51.250.87.170 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

Проверка аптайма

```bash
ansible all -i '51.250.87.170,' -u dima --private-key ~/.ssh/yacloud -a 'uptime'

51.250.87.170 | CHANGED | rc=0 >>
 19:22:25 up 47 min,  1 user,  load average: 0.00, 0.00, 0.00
```

Добавил файл inventory.ini, хотя в документации написано что это можно делать в формате yaml но видимо про это будет в следующих уроках.

В файле инвентаря для проброски пути до нужного ssh ключа используется параметр `ansible_ssh_private_key_file`, теперь команду можно выполнять так:

```bash
ansible all -i inventory.ini -u dima -a 'uptime'

51.250.87.170 | CHANGED | rc=0 >>
 19:29:26 up 54 min,  1 user,  load average: 0.04, 0.01, 0.00
```
