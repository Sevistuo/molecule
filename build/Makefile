USER := retr0h
TAG := $(shell git describe --tags `git rev-list --tags --max-count=1`)
LATEST_IMAGE := $(USER)/molecule:latest
TAG_IMAGE := $(USER)/molecule:$(TAG)

clean:
	@echo "+ $@"
	rm -rf build/lib/
	rm -rf dist/

build: clean
	@echo "+ $@"
	git clean -d -x -f molecule/cookiecutter/
	./.tox/py27-ansible25-unit/bin/python setup.py sdist bdist_wheel
	./.tox/py36-ansible25-unit/bin/python setup.py sdist bdist_wheel

docker-build:
	@echo "+ $@"
	cd build && sudo docker build \
		--rm=true \
		--no-cache \
		-t $(LATEST_IMAGE) -t $(TAG_IMAGE) .

push:
	./.tox/py27-ansible25-unit/bin/twine upload dist/* -r pypi

docker-push:
	@echo "+ $@"
	sudo docker push $(LATEST_IMAGE)
	sudo docker push $(TAG_IMAGE)

.PHONY: build
