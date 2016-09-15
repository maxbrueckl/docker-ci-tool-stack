FROM zsoltm/debian-armhf:jessie

COPY assets/ app/

RUN apt-get update \
	&& apt-get install curl openssh-server ca-certificates postfix apt-transport-https locales \
	&& rm -rf /var/lib/apt/lists/*
RUN curl https://packages.gitlab.com/gpg.key | apt-key add -
RUN curl -sS https://packages.gitlab.com/install/repositories/gitlab/raspberry-pi2/script.deb.sh | sudo bash
RUN apt-get update \
	&& apt-get install gitlab-ce \
	&& update-locale LANG=C.UTF-8 LC_MESSAGES=POSIX \
	&& locale-gen en_US.UTF-8 \
	&& apt-get clean \
	&& rm -rf /var/lib/apt/lists/*
RUN gitlab-ctl reconfigure

COPY entrypoint.sh /sbin/entrypoint.sh
RUN chmod 755 /sbin/entrypoint.sh

EXPOSE 22/tcp 80/tcp 443/tcp

VOLUME ["${GITLAB_DATA_DIR}", "${GITLAB_LOG_DIR}"]
WORKDIR ${GITLAB_INSTALL_DIR}
ENTRYPOINT ["/sbin/entrypoint.sh"]
CMD ["app:start"]