REGISTRY?=luxas
IMAGE?=traefik
TEMP_DIR:=$(shell mktemp -d)
ARCH?=amd64
ALL_ARCH=amd64 arm arm64
ML_PLATFORMS=linux/amd64,linux/arm,linux/arm64

VERSION?=v1.3.8
URL?=https://github.com/containous/traefik/releases/download/$(VERSION)/traefik_linux-$(ARCH)

all: build
build:
	curl -sSL $(URL) > $(TEMP_DIR)/traefik
	chmod +x $(TEMP_DIR)/traefik
	cp Dockerfile $(TEMP_DIR)

	docker build -t $(REGISTRY)/$(IMAGE)-$(ARCH):$(VERSION) $(TEMP_DIR)
	rm -r $(TEMP_DIR)

push-%:
	$(MAKE) ARCH=$* build
	docker push $(REGISTRY)/$(IMAGE)-$*:$(VERSION)

push: ./manifest-tool $(addprefix push-,$(ALL_ARCH))
	./manifest-tool push from-args --platforms $(ML_PLATFORMS) --template $(REGISTRY)/$(IMAGE)-ARCH:$(VERSION) --target $(REGISTRY)/$(IMAGE):$(VERSION)

./manifest-tool:
	curl -sSL https://github.com/estesp/manifest-tool/releases/download/v0.7.0/manifest-tool-linux-amd64 > manifest-tool
	chmod +x manifest-tool
