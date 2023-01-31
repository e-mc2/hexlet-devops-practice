.PHONY: play install

install:
	ansible-galaxy install -r requirements.yml

play:
	ansible-playbook --ask-vault-pass play.yml
