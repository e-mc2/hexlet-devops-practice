Скопировал инвентарь из предыдущего упражнения, сделал playbook по установке git и добавил на это make команду: `install`

```bash
make install

ansible-playbook playbook_install_git.yml -i inventory.ini

PLAY [services] ***********************************************************************************

TASK [Gathering Facts] ****************************************************************************
ok: [yandex]
ok: [ec2-54-205-30-45.compute-1.amazonaws.com]
ok: [ec2-54-172-152-108.compute-1.amazonaws.com]

TASK [install git] ********************************************************************************
ok: [yandex]
ok: [ec2-54-205-30-45.compute-1.amazonaws.com]
ok: [ec2-54-172-152-108.compute-1.amazonaws.com]

PLAY RECAP ****************************************************************************************
ec2-54-172-152-108.compute-1.amazonaws.com : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ec2-54-205-30-45.compute-1.amazonaws.com : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
yandex                     : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

Зашел на сервер по ssh и проверил установленный git

```bash
ssh -i ~/.ssh/aws ubuntu@ec2-54-172-152-108.compute-1.amazonaws.com

ubuntu@ip-172-31-89-63:~$ git --version
git version 2.34.1
```

Сделал playbook по удалению git и добавил на это make команду: `delete`

```bash
make delete

ansible-playbook playbook_delete_git.yml -i inventory.ini

PLAY [services] ***********************************************************************************

TASK [Gathering Facts] ****************************************************************************
ok: [yandex]
ok: [ec2-54-172-152-108.compute-1.amazonaws.com]
ok: [ec2-54-205-30-45.compute-1.amazonaws.com]

TASK [delete git] *********************************************************************************
changed: [yandex]
changed: [ec2-54-172-152-108.compute-1.amazonaws.com]
changed: [ec2-54-205-30-45.compute-1.amazonaws.com]

PLAY RECAP ****************************************************************************************
ec2-54-172-152-108.compute-1.amazonaws.com : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ec2-54-205-30-45.compute-1.amazonaws.com : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
yandex                     : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

Зашел на сервер по ssh и проверил что git удален

```bash
ssh -i ~/.ssh/aws ubuntu@ec2-54-172-152-108.compute-1.amazonaws.com

ubuntu@ip-172-31-89-63:~$ git --version
Command 'git' not found, but can be installed with:
sudo apt install git
```

МЫСЛИ:
- Зачем было переименовать в плейбуках команды apt-get не понятно.. то есть для каждой команды я должен буду искать соответствующий эквивалент в ansible - оооочень не удобно =(
