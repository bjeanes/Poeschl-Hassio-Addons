ARG BUILD_FROM
FROM $BUILD_FROM AS BUILD

RUN apk add --no-cache \
	'git' \
	'make'

ENV GIT_SECRET_VERSION 1.3.0
# hadolint ignore=DL3003
RUN git config --global advice.detachedHead false && \
	git clone https://github.com/awslabs/git-secrets.git -b ${GIT_SECRET_VERSION} /git-secrets && \
	cd /git-secrets && make install

FROM $BUILD_FROM AS RUNNING

RUN apk add --no-cache \
	'git' \
	'rsync' \
	'grep' \
	'sed' \
	'python3' \
	'py3-pip' \
	'findutils' \
	&& pip3 install --no-cache-dir PyYAML==6.0 dotty-dict==1.3.0

COPY --from=BUILD /usr/local/bin/git-secrets /usr/local/bin/git-secrets

COPY root /
RUN chmod a+x /run.sh /utils/*
ENTRYPOINT [ "/run.sh" ]
