.PHONY: users init nginx

users:
	ansible-playbook playbook.yml --limit appservers -i inventory.ini -t users

init:
	ansible-playbook playbook.yml --limit appservers -i inventory.ini -t init

nginx:
	ansible-playbook playbook.yml --limit appservers -i inventory.ini -t nginx
