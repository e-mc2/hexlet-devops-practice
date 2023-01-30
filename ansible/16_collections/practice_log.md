При попытке запустить команду `ansible-galaxy collection list` получаю ошибку:

```
ERROR! - None of the provided paths were usable. Please specify a valid path with --collections-path
```

Пробовал указать переменную `collections-path` в настройках **ansible** в ~/.ansible.cfg но не помогло..
Команда заработала уже после установки коллекции через **ansible-galaxy**:

```
ansible-galaxy collection install nginxinc.nginx_core
```

После установки первой не встроенной коллекции появилась директория `/.ansible/collections/ansible_collections` и начала работать команда `collection list`

---

Помимо установки пользователей решил попробовать установить и nginx с помощью коллекции ` nginxinc.nginx_core`, не самое конечно очевидное использование, после того как делаешь все шаги сам, нашел [примеры](https://github.com/nginxinc/ansible-collection-nginx/tree/main/playbooks) использования у них в репозитории, хотя в самой документации (readme) про это не слова. Но прямое использование с дефолтными настройками не взлетает.. ни установка ни применение конфига, буду уже пробовать отдельно вне рамках этого задания

Также заметил что не пробрасывается порт nginx при таком использовании шаблона =(
