FROM alpine AS builder
MAINTAINER José Ribeiro <hello@jlbribeiro.com>

RUN echo "@testing http://dl-cdn.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories \
 && apk update

RUN apk add --no-cache \
	make \
	cmake \
	libgcc \
	libstdc++ \
	g++ \
	boost-filesystem \
	boost-system \
	boost-thread \
	boost-dev \
	expat \
	expat-dev \
	zlib-dev \
	libbz2 \
	bzip2-dev \
	libpq \
	postgresql-libs \
	postgresql-dev \
	proj4@testing \
	proj4-dev@testing \
	geos@testing \
	geos-dev@testing \
	lua5.2-dev \
	git

ARG OSM2PGSQL_VERSION=0.96.0

RUN mkdir src && cd src \
     && git clone --depth 1 --branch $OSM2PGSQL_VERSION https://github.com/openstreetmap/osm2pgsql.git \
     && cd osm2pgsql \
	 && mkdir build && cd build \
             && cmake \
		-DLUA_INCLUDE_DIR=/usr/include/lua5.2/ \
		-DLUA_LIBRARY=/usr/lib/liblua-5.2.so.0 \
		.. \
             && make && make install \
	     && cd ../../../ \
 && rm -rf src


FROM scratch

COPY --from=builder /lib/ld-musl-x86_64.so.1    /lib/ld-musl-x86_64.so.1
COPY --from=builder /usr/lib/libpq.so.5         /usr/lib/libpq.so.5
COPY --from=builder /lib/libz.so.1              /lib/libz.so.1
COPY --from=builder /usr/lib/libexpat.so.1      /usr/lib/libexpat.so.1
COPY --from=builder /usr/lib/libbz2.so.1        /usr/lib/libbz2.so.1
COPY --from=builder /usr/lib/libproj.so.13      /usr/lib/libproj.so.13
COPY --from=builder /usr/lib/libstdc++.so.6     /usr/lib/libstdc++.so.6
COPY --from=builder /usr/lib/libgcc_s.so.1      /usr/lib/libgcc_s.so.1
COPY --from=builder /lib/ld-musl-x86_64.so.1    /lib/ld-musl-x86_64.so.1
COPY --from=builder /lib/libssl.so.45           /lib/libssl.so.45
COPY --from=builder /lib/libcrypto.so.43        /lib/libcrypto.so.43
COPY --from=builder /usr/lib/libldap_r-2.4.so.2 /usr/lib/libldap_r-2.4.so.2
COPY --from=builder /usr/lib/liblber-2.4.so.2   /usr/lib/liblber-2.4.so.2
COPY --from=builder /usr/lib/libsasl2.so.3      /usr/lib/libsasl2.so.3

COPY --from=builder /usr/lib/lua5.2/ /usr/lib/lua5.2/
COPY --from=builder /usr/lib/liblua-5.2.so.0 /usr/lib/liblua-5.2.so.0

COPY --from=builder /usr/local/bin/osm2pgsql              /usr/local/bin/osm2pgsql
COPY --from=builder /usr/local/share/osm2pgsql/           /usr/local/share/osm2pgsql/
COPY --from=builder /usr/local/share/man/man1/osm2pgsql.1 /usr/local/share/man/man1/osm2pgsql.1

ENTRYPOINT ["/usr/local/bin/osm2pgsql"]
