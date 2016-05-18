.PHONY: docker-start docker-stop all

DEBUG =
TAGS = all

all: executable

docker-start: TAGS = docker-start
docker-start: executable

docker-stop: TAGS = docker-stop
docker-stop: executable

debug: DEBUG = -vvvv
debug: executable

executable:
	ansible-playbook -i inventory -t $(TAGS) test.yml $(DEBUG)
