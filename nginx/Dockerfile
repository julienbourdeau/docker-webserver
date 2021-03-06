FROM nginx:1
LABEL maintainer "Nazar Mokrynskyi <nazar@mokrynskyi.com>"

RUN \

	apt-get update && \
	apt-get upgrade -y && \

# Ceph repository for up to date version of ceph-fuse package; Ceph itself is used as cluster filesystem

	CEPH_VERSION=kraken && \
	apt-get install -y --no-install-recommends curl ca-certificates && \
	curl -sSL 'https://download.ceph.com/keys/release.asc' | apt-key add - && \
	echo "deb http://download.ceph.com/debian-$CEPH_VERSION/ jessie main" > /etc/apt/sources.list.d/ceph-$CEPH_VERSION.list && \
	apt-get update && \
	apt-get install -y --no-install-recommends ceph-fuse && \
	apt-get purge -y --auto-remove curl ca-certificates && \

	apt-get clean && \
	rm -rf /var/lib/apt/lists/* && \

# Change Nginx user

	sed -i 's/user  nginx;/user  git;/g' /etc/nginx/nginx.conf && \

# Make Nginx work with PHP with default config

	sed -i 's/127.0.0.1:9000/php:9000/g' /etc/nginx/conf.d/default.conf && \

# Create group and user for Nginx

	addgroup -gid 1000 git && \
	useradd -d /data -s /bin/bash -g 1000 -u 1000 git && \

	mv /etc/nginx /etc/nginx_dist

COPY ceph-mount.sh /
COPY webserver-entrypoint.sh /

VOLUME \
	/data \
	/etc/nginx \
	/usr/share/nginx/html \
	/var/log/nginx

ENV \
	CEPH_MON_SERVICE=ceph-mon \
	CEPHFS_MOUNT=0

ENTRYPOINT ["/webserver-entrypoint.sh"]

CMD ["nginx", "-g", "daemon off;"]
