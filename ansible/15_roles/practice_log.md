Прикольно.. в уроке расписано как пользоваться ролями размещенными на galaxy, а в задании просят создать собственную роль - а это вообще совершенно другой мир со своими правилами, иерархиями папок и др, ну что ж будем читать мат часть

> 3. Создайте роль для настройки окружения пользователем. Эта роль состоит из двух задач: создание пользователя и добавление конфига для него. Если пользователь удаляется, то генерировать конфиг Git не нужно

Что значит, `Если пользователь удаляется`? Про какой момент речь?  Мы делаем роль из двух тасок - для создания пользователя(ей), ничего не удаляется в этом процессе, не понятно `\_(ツ)_/¯`

Собрал две роли из предыдущих заданий: add_users и git, вторая в зависимостях у первой, пользователей создаю в цикле из переменных в основной плейбуке

```
ansible-playbook playbook.yml -i inventory.ini                             ✔  12s    3.10.7   

PLAY [Trying roles] *******************************************************************************************************************************************

TASK [Gathering Facts]
************************************************************************************************************
ok: [yandex]

TASK [git : Install git]
************************************************************************************************************
ok: [yandex]

TASK [add_users : Add users]
************************************************************************************************************
ok: [yandex] => (item={'name': 'jaime', 'email': 'jaime@example.com'})
ok: [yandex] => (item={'name': 'sansa', 'email': 'sansa@example.com'})
ok: [yandex] => (item={'name': 'robert', 'email': 'robert@example.com'})

TASK [add_users : Create gitconfig for users] ************************************************************************************************************
changed: [yandex] => (item={'name': 'jaime', 'email': 'jaime@example.com'})
changed: [yandex] => (item={'name': 'sansa', 'email': 'sansa@example.com'})
changed: [yandex] => (item={'name': 'robert', 'email': 'robert@example.com'})

TASK [add_users : Create dir .ssh for each users]
************************************************************************************************************
changed: [yandex] => (item={'name': 'jaime', 'email': 'jaime@example.com'})
changed: [yandex] => (item={'name': 'sansa', 'email': 'sansa@example.com'})
changed: [yandex] => (item={'name': 'robert', 'email': 'robert@example.com'})

TASK [add_users : Copy public key for each users]
************************************************************************************************************
changed: [yandex] => (item={'name': 'jaime', 'email': 'jaime@example.com'})
changed: [yandex] => (item={'name': 'sansa', 'email': 'sansa@example.com'})
changed: [yandex] => (item={'name': 'robert', 'email': 'robert@example.com'})

PLAY RECAP **************************************************************************************************
yandex                     : ok=6    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

Проверка

```
ssh -i ~/.ssh/yacloud sansa@51.250.64.213
Welcome to Ubuntu 22.04.1 LTS (GNU/Linux 5.15.0-56-generic x86_64)

$ pwd
/home/sansa
$ git init
hint: Using 'master' as the name for the initial branch. This default branch name
hint: is subject to change. To configure the initial branch name to use in all
hint: of your new repositories, which will suppress this warning, call:
hint:
hint:   git config --global init.defaultBranch <name>
hint:
hint: Names commonly chosen instead of 'master' are 'main', 'trunk' and
hint: 'development'. The just-created branch can be renamed via this command:
hint:
hint:   git branch -m <name>
Initialized empty Git repository in /home/sansa/.git/
$ git st
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .bash_logout
        .bashrc
        .cache/
        .gitconfig
        .profile
        .ssh/

nothing added to commit but untracked files present (use "git add" to track)
```
