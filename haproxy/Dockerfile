FROM haproxy:1
LABEL maintainer "Nazar Mokrynskyi <nazar@mokrynskyi.com>"

RUN \
	apt-get update && \
	apt-get upgrade -y && \
	apt-get install -y --no-install-recommends dnsutils && \
	apt-get clean && \
	rm -rf /var/lib/apt/lists/*


COPY haproxy.conf /usr/local/etc/haproxy/haproxy.conf.dist
COPY webserver-entrypoint.sh /

ENTRYPOINT ["/webserver-entrypoint.sh"]
