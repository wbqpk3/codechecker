THRIFT_OPTS = -r -o /data
THRIFT_DOCKER_IMAGE = "thrift_compiler"
TARGET_PY = --gen py
TARGET_JS = --gen js:node

API_DIR := $(CURDIR)
uid = $(shell id -u)
guid = $(shell id -g)

clean_stubs:
	rm -rf $(API_DIR)/python/ $(API_DIR)/js/ $(API_DIR)/gen-*/

thrift_docker:
	@echo "Building Thrift docker image ..."
	docker build -t $(THRIFT_DOCKER_IMAGE) -f Dockerfile .

thrift_stubs: clean_stubs
	@echo "Generating Thrift Python and JS stubs ..."
	docker run \
	  --rm \
	  -u "$(uid):$(guid)" \
	  -v $(API_DIR):/data \
	  ${THRIFT_DOCKER_IMAGE} \
	  /bin/bash -c " \
		thrift $(THRIFT_OPTS) $(TARGET_PY) $(TARGET_JS) /data/authentication.thrift && \
		thrift $(THRIFT_OPTS) $(TARGET_PY) $(TARGET_JS) /data/configuration.thrift  && \
		thrift $(THRIFT_OPTS) $(TARGET_PY) $(TARGET_JS) /data/products.thrift       && \
		thrift $(THRIFT_OPTS) $(TARGET_PY) $(TARGET_JS) /data/report_server.thrift  && \
		thrift $(THRIFT_OPTS) $(TARGET_PY) $(TARGET_JS) /data/server_info.thrift    && \
		thrift $(THRIFT_OPTS) $(TARGET_PY) $(TARGET_JS) /data/tasks.thrift"

	@echo "Creating codechecker_api package layout ..."
	mv $(API_DIR)/gen-py/codechecker_api/python/ $(API_DIR)
	rm -rf $(API_DIR)/gen-py/
	mv $(API_DIR)/gen-nodejs/ $(API_DIR)/js/

build_codechecker_api:
	docker image inspect ${THRIFT_DOCKER_IMAGE} >/dev/null 2>&1 && \
		echo "Thrift Docker image exists." || \
		(echo "Thrift Docker image not found." && $(MAKE) thrift_docker)
	$(MAKE) thrift_stubs
