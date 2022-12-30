Вообще конечно странный урок, так как если следовать ему напрямую то ничего не будет работать. Но если это было так задумано то тогда ок :)

Playbook из урока работать не будет:

```
- hosts: webservers
  tasks:
    - name: update nginx config
      ansible.builtin.template:
        src: templates/nginx.conf.j2
        dest: /var/tmp/www/nginx.conf
```

потому что настройки nginx лежат в другом месте, а именно `/etc/nginx/nginx.conf` в видео вообще какой-то старый формат yaml

еще столкнулся с проблемой, что до инстансов поднятых на амазоне нельзя достучаться через браузер или иным образом кроме как через ssh:

> To allow traffic on port 80 and 443, you must configure the associated security group and network access control list (network ACL).

Решается через добавление inbound в security group, NACL не настраивал - запустилось и так.

