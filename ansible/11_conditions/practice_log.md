Посмотрел какие факты можно достать с виртуальных машин с помощью модуля setup:

```
ansible all --limit yandex-centos -i inventory.ini -m setup | less
```

И решил использовать свойство `ansible_pkg_mgr` - явное определение какой менеджер пакетов есть на машине:

```
PLAY [Install git for different systems] *********************************************

TASK [Gathering Facts] ***************************************************************
ok: [yandex-centos]
ok: [yandex-ubuntu]

TASK [Install git for Ubuntu] ********************************************************
skipping: [yandex-centos]
ok: [yandex-ubuntu]

TASK [Install git for Centos] ********************************************************
skipping: [yandex-ubuntu]
changed: [yandex-centos]

PLAY RECAP ***************************************************************************
yandex-centos              : ok=2    changed=1    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
yandex-ubuntu              : ok=2    changed=0    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
```
