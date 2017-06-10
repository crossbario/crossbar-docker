default:
	@echo ""
	@echo "Targets:"
	@echo ""
	@echo "  params                 Print build parameter"
	@echo "  build                  Build images"
	@echo "  version                Print version of images"
	@echo "  test                   Test images"
	@echo "  publish                Publish images"
	@echo ""


params:
	@echo "${CROSSBAR_VERSION}"
	@echo "${CROSSBAR_VCS_REF}"
	@echo "${BUILD_DATE}"

build: build_cpy3 build_pypy3

version: version_cpy3 version_pypy3

test: test_cpy3 test_pypy3


publish: build
	# amd64 / community / cpy3
	docker push crossbario/crossbar:latest
	docker push crossbario/crossbar:cpy3
	docker push crossbario/crossbar:cpy3-${CROSSBAR_VERSION}

	# armhf / community / cpy3
	docker push crossbario/crossbar-armhf:latest
	docker push crossbario/crossbar-armhf:cpy3
	docker push crossbario/crossbar-armhf:cpy3-${CROSSBAR_VERSION}

	# aarch64 / community / cpy3
	docker push crossbario/crossbar-aarch64:latest
	docker push crossbario/crossbar-aarch64:cpy3
	docker push crossbario/crossbar-aarch64:cpy3-${CROSSBAR_VERSION}

	# amd64 / community / pypy3
	docker push crossbario/crossbar:pypy3
	docker push crossbario/crossbar:pypy3-${CROSSBAR_VERSION}

	# trigger updates of metainfo on https://microbadger.com/
	curl -X POST https://hooks.microbadger.com/images/crossbario/crossbar/jA-pM9fXwd6v4Q7M3PWGG10XPAE=
	curl -X POST https://hooks.microbadger.com/images/crossbario/crossbar-armhf/3hB5Fn-4Bf2VV9ihB8e7v687Y60=
	curl -X POST https://hooks.microbadger.com/images/crossbario/crossbar-aarch64/ekMu3620yWLtfpxIqJQgPSmDNYE=


#
# Copy over current AutobahnJS into the default node directory
# we bake into the images
#
autobahn:
	cp ../../autobahn-js-built/autobahn.* ./node/web/js/
	cp ../../autobahn-js-built/CHECKSUM.* ./node/web/js/
	cp ../../autobahn-js-built/LICENSE ./node/web/js/

#
# List all images
#
list:
	-docker images crossbario/crossbar*

#
# Remove all images !!!
#
clean:
	./removeall.sh



#
# BUILD: Community / CPython 3
#
build_cpy3: build_amd64_cpy3 build_armhf_cpy3 build_aarch64_cpy3

build_amd64_cpy3:
	# enforce auto-generating a new key when using the images
	rm -f node/.crossbar/key.*
	time docker build \
		--build-arg BUILD_DATE=${BUILD_DATE} \
		--build-arg CROSSBAR_VCS_REF=${CROSSBAR_VCS_REF} \
		--build-arg CROSSBAR_VERSION=${CROSSBAR_VERSION} \
		-t crossbario/crossbar:latest \
		-t crossbario/crossbar:cpy3 \
		-t crossbario/crossbar:cpy3-${CROSSBAR_VERSION} \
		-f Dockerfile.cpy3 .

build_armhf_cpy3:
	# enforce auto-generating a new key when using the images
	rm -f node/.crossbar/key.*
	time docker build \
		--build-arg BUILD_DATE=${BUILD_DATE} \
		--build-arg CROSSBAR_VCS_REF=${CROSSBAR_VCS_REF} \
		--build-arg CROSSBAR_VERSION=${CROSSBAR_VERSION} \
		--build-arg IMAGE_PREFIX=arm32v7/ \
		-t crossbario/crossbar-armhf:latest \
		-t crossbario/crossbar-armhf:cpy3 \
		-t crossbario/crossbar-armhf:cpy3-${CROSSBAR_VERSION} \
		-f Dockerfile.cpy3 .

build_aarch64_cpy3:
	# enforce auto-generating a new key when using the images
	rm -f node/.crossbar/key.*
	time docker build \
		--build-arg BUILD_DATE=${BUILD_DATE} \
		--build-arg CROSSBAR_VCS_REF=${CROSSBAR_VCS_REF} \
		--build-arg CROSSBAR_VERSION=${CROSSBAR_VERSION} \
		--build-arg IMAGE_PREFIX=aarch64/ \
		-t crossbario/crossbar-aarch64:latest \
		-t crossbario/crossbar-aarch64:cpy3 \
		-t crossbario/crossbar-aarch64:cpy3-${CROSSBAR_VERSION} \
		-f Dockerfile.cpy3 .


#
# BUILD: Community / PyPy 3
#
build_pypy3: build_amd64_pypy3 build_armhf_pypy3 build_aarch64_pypy3

build_amd64_pypy3:
	# enforce auto-generating a new key when using the images
	rm -f node/.crossbar/key.*
	time docker build \
		--build-arg BUILD_DATE=${BUILD_DATE} \
		--build-arg CROSSBAR_VCS_REF=${CROSSBAR_VCS_REF} \
		--build-arg CROSSBAR_VERSION=${CROSSBAR_VERSION} \
		--build-arg PYPY_VERSION=5.8.0 \
		--build-arg PYPY_SHA256SUM=57d871a7f1135719c138cee4e3533c3275d682a76a40ff668e95150c65923035 \
		-t crossbario/crossbar:pypy3 \
		-t crossbario/crossbar:pypy3-${CROSSBAR_VERSION} \
		-f Dockerfile.pypy3 .

build_armhf_pypy3:
	@echo "PyPy3 isn't available for armhf yet"

build_aarch64_pypy3:
	@echo "PyPy3 isn't available for aarch64 yet"


#
# VERSION: Community / CPython 3
#
version_cpy3: version_amd64_cpy3 version_armhf_cpy3 version_aarch64_cpy3

version_amd64_cpy3:
	docker run \
		--rm --entrypoint=/usr/local/bin/crossbar -it \
		crossbario/crossbar:cpy3 version

version_armhf_cpy3:
	docker run \
		--rm --entrypoint=/usr/local/bin/crossbar -it \
		crossbario/crossbar-armhf:cpy3 version

version_aarch64_cpy3:
	docker run \
		--rm --entrypoint=/usr/local/bin/crossbar -it \
		crossbario/crossbar-aarch64:cpy3 version


#
# VERSION: Community / PyPy 3
#
version_pypy3: version_amd64_pypy3 version_armhf_pypy3 version_aarch64_pypy3

version_amd64_pypy3:
	docker run \
		--rm --entrypoint=/usr/local/bin/crossbar -it \
		crossbario/crossbar:pypy3 version

version_armhf_pypy3:
	@echo "PyPy3 isn't available for armhf yet"

version_aarch64_pypy3:
	@echo "PyPy3 isn't available for aarch64 yet"


#
# TEST: Community / CPython 3
#
test_cpy3: test_amd64_cpy3 test_armhf_cpy3 test_aarch64_cpy3

test_amd64_cpy3:
	docker run \
		--rm -it -p 8080:8080 --name crossbar \
		crossbario/crossbar:cpy3

test_armhf_cpy3:
	docker run \
		--rm -it -p 8080:8080 --name crossbar \
		crossbario/crossbar-armhf:cpy3

test_aarch64_cpy3:
	docker run \
		--rm -it -p 8080:8080 --name crossbar \
		crossbario/crossbar-aarch64:cpy3


#
# TEST: Community / PyPy 3
#
test_pypy3: test_amd64_pypy3 test_armhf_pypy3 test_aarch64_pypy3

test_amd64_pypy3:
	docker run \
		--rm -it -p 8080:8080 --name crossbar \
		crossbario/crossbar:pypy3

test_armhf_pypy3:
	@echo "PyPy3 isn't available for armhf yet"

test_aarch64_pypy3:
	@echo "PyPy3 isn't available for aarch64 yet"

