FROM nazarpc/php:fpm
LABEL maintainer "Nazar Mokrynskyi <nazar@mokrynskyi.com>"

RUN \

	apt-get update && \
	apt-get upgrade -y && \

# Ceph repository for up to date version of ceph-fuse package; Ceph itself is used as cluster filesystem

	CEPH_VERSION=kraken && \
	curl -sSL 'https://download.ceph.com/keys/release.asc' | apt-key add - && \
	echo "deb http://download.ceph.com/debian-$CEPH_VERSION/ jessie main" > /etc/apt/sources.list.d/ceph-$CEPH_VERSION.list && \
	apt-get update && \
	apt-get install -y --no-install-recommends ceph-fuse && \

	apt-get clean && \
	rm -rf /var/lib/apt/lists/* && \

# Change PHP-FPM user

	sed -i 's/www-data/git/g' /usr/local/etc/php-fpm.d/www.conf && \

# Create group and user for PHP-FPM

	addgroup -gid 1000 git && \
	useradd -d /data -s /bin/bash -g 1000 -u 1000 git && \

	mv /usr/local/etc /usr/local/etc_dist

COPY ceph-mount.sh /
COPY webserver-entrypoint.sh /

VOLUME \
	/data \
	/usr/local/etc

ENV \
	CEPH_MON_SERVICE=ceph-mon \
	CEPHFS_MOUNT=0

ENTRYPOINT ["/webserver-entrypoint.sh"]

CMD ["php-fpm"]
