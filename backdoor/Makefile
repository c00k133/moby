# This Makefile is meant to be run inside a Vagrant machine.
# You can find the corresponding Vagrant file in the root of this project.

DOCKER_TAG=backdoor

.PHONY: install
install: daemon
	$(MAKE) --directory=.. binary
	@sudo $(MAKE) --directory=.. install

.PHONY: build
build:
	@docker build --no-cache --tag $(DOCKER_TAG) --file Dockerfile ..

.PHONY: sh
sh:
	@docker run --interactive --tty $(DOCKER_TAG) sh

.PHONY: dockerd
dockerd:
	@sudo systemctl stop docker
	@sudo systemctl stop docker.socket
	@sudo dockerd --debug=true

.PHONY: daemon
daemon:
	@sudo systemctl start docker
	@sudo systemctl start docker.socket
