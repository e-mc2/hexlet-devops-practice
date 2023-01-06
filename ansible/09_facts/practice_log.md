Попробовал модуль setup для сервера в яндекс облаке:

```bash
ansible all -i inventory.ini -m setup | less

yandex1 | SUCCESS => {
    "ansible_facts": {
        "ansible_all_ipv4_addresses": [
            "10.128.0.6"
        ],
        "ansible_all_ipv6_addresses": [
            "fe80::d20d:acff:fe1d:481"
        ],
        "ansible_apparmor": {
            "status": "enabled"
        },
        "ansible_architecture": "x86_64",
        "ansible_bios_date": "04/01/2014",
        "ansible_bios_vendor": "SeaBIOS",
        "ansible_bios_version": "1.11.1-1",
        "ansible_board_asset_tag": "NA",
        "ansible_board_name": "xeon-gold-6230",
        "ansible_board_serial": "NA",
        "ansible_board_vendor": "Yandex",
        "ansible_board_version": "pc-q35-yc-2.12",
        "ansible_chassis_asset_tag": "NA",
        "ansible_chassis_serial": "NA",
        "ansible_chassis_vendor": "Yandex",
        "ansible_chassis_version": "xeon-gold-6230",
        "ansible_cmdline": {
            "BOOT_IMAGE": "/boot/vmlinuz-5.15.0-56-generic",
            "biosdevname": "0",
            "console": "ttyS0",
            "net.ifnames": "0",
            "ro": true,
            "root": "UUID=82aeea96-6d42-49e6-85d5-9071d3c9b6aa"
...
```

Плейбук на установку git cо сбором информации о целевой машине:

```bash
ansible-playbook -i inventory.ini playbook.yml

PLAY [all] *****************************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [yandex1]

TASK [install git] *********************************************************************************************************
ok: [yandex1]

PLAY RECAP *****************************************************************************************************************
yandex1                    : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

Avg. time: 14s
```

Плейбук на установку git без сбора фактов:

```bash
ansible-playbook -i inventory.ini playbook.yml

PLAY [all] *****************************************************************************************************************

TASK [install git] *********************************************************************************************************
ok: [yandex1]

PLAY RECAP *****************************************************************************************************************
yandex1                    : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

Avg. time: 10s
```

Делал через мобильный интернет и в среднем (запускал по 5 раз) без фактов выполнялось 10 сек против 14 сек с фактами
