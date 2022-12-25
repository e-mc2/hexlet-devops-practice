Для наглядности помимо инстанса на Yandex Cloud добавил еще два на AWS. Добавил адреса серверов в inventory файл по аналогии с теорией в этом упражнении

Но после выполнения команды

```bash
ansible all -i inventory.ini -u root -m ping
```

Получил ошибку

```bash
51.250.87.170 | UNREACHABLE! => {
    "changed": false,
    "msg": "Failed to create temporary directory. In some cases, you may have been able to authenticate and did not have permissions on the target directory. Consider changing the remote tmp path in ansible.cfg to a path rooted in \"/tmp\", for more error information use -vvv. Failed command was: ( umask 77 && mkdir -p \"` echo Please login as the user \"NONE\" rather than the user \"root\"./.ansible/tmp `\"&& mkdir \"` echo Please login as the user \"NONE\" rather than the user \"root\"./.ansible/tmp/ansible-tmp-1671978114.794234-3482-199200046650880 `\" && echo ansible-tmp-1671978114.794234-3482-199200046650880=\"` echo Please login as the user \"NONE\" rather than the user \"root\"./.ansible/tmp/ansible-tmp-1671978114.794234-3482-199200046650880 `\" ), exited with result 142, stdout output: Please login as the user \"NONE\" rather than the user \"root\".\n\n",
    "unreachable": true
}
ec2-54-205-30-45.compute-1.amazonaws.com | UNREACHABLE! => {
    "changed": false,
    "msg": "Failed to create temporary directory. In some cases, you may have been able to authenticate and did not have permissions on the target directory. Consider changing the remote tmp path in ansible.cfg to a path rooted in \"/tmp\", for more error information use -vvv. Failed command was: ( umask 77 && mkdir -p \"` echo Please login as the user \"ubuntu\" rather than the user \"root\"./.ansible/tmp `\"&& mkdir \"` echo Please login as the user \"ubuntu\" rather than the user \"root\"./.ansible/tmp/ansible-tmp-1671978119.018767-3480-53999261394359 `\" && echo ansible-tmp-1671978119.018767-3480-53999261394359=\"` echo Please login as the user \"ubuntu\" rather than the user \"root\"./.ansible/tmp/ansible-tmp-1671978119.018767-3480-53999261394359 `\" ), exited with result 142, stdout output: Please login as the user \"ubuntu\" rather than the user \"root\".\n\n",
    "unreachable": true
}
```

Да на сервере в яндексе у меня пользователь `dima` но на амазоновских серверах я ничего не создавал дополнительно (у яндекса панель кстати удобнее в этом плане), из ошибки понятно под каким пользователем сервисы советуют подключаться.

Интересно а под root'ом вообще нельзя подключиться?

Оказывается по умолчанию этого сделать нельзя, но можно отдельно включить эту опцию в файле `/etc/ssh/sshd_config` -> `PermitRootLogin yes` и перезапустить сервис ssh `systemctl restart ssh || systemctl restart sshd`.

В файле inventory для каждой машины указал ansible_user + ansible_ssh_private_key_file (для ключей ssh)

```bash
ansible all -i inventory.ini -m ping

51.250.87.170 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
ec2-54-172-152-108.compute-1.amazonaws.com | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
ec2-54-205-30-45.compute-1.amazonaws.com | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

Для сервиса на яндексе задал имя yandex:

```bash
ansible all --limit yandex -i inventory.ini -a 'uptime'

yandex | CHANGED | rc=0 >>
 15:01:49 up 1 day, 20:27,  1 user,  load average: 0.00, 0.00, 0.00
```

Попробовал группировку сервисов через children

```bash
ansible services -i inventory.ini -a 'uptime'

yandex | CHANGED | rc=0 >>
 15:03:19 up 1 day, 20:28,  1 user,  load average: 0.00, 0.00, 0.00
ec2-54-205-30-45.compute-1.amazonaws.com | CHANGED | rc=0 >>
 15:03:23 up 46 min,  1 user,  load average: 0.00, 0.00, 0.00
ec2-54-172-152-108.compute-1.amazonaws.com | CHANGED | rc=0 >>
 15:03:23 up  1:00,  1 user,  load average: 0.00, 0.00, 0.00
```
