.PHONY: all
all: build

OSM2PGSQL_VERSION=0.96.0

.PHONY: build
build:
	docker build \
		--build-arg OSM2PGSQL_VERSION=${OSM2PGSQL_VERSION} \
		-t jlbribeiro/osm2pgsql:${OSM2PGSQL_VERSION} \
		-t jlbribeiro/osm2pgsql:latest \
		.

.PHONY: run
run:
	docker run -it --rm jlbribeiro/osm2pgsql:latest
