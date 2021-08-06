FROM openjdk:8u252-jre

MAINTAINER suyongjie

# debian：buster(10)
RUN echo "deb http://mirrors.aliyun.com/debian/ buster main non-free contrib" > /etc/apt/sources.list \
 && echo "deb http://mirrors.aliyun.com/debian-security buster/updates main" >> /etc/apt/sources.list \
 && echo "deb http://mirrors.aliyun.com/debian/ buster-updates main non-free contrib" >> /etc/apt/sources.list 

COPY ./requirements.txt /usr/local/src/requirements.txt

RUN set -eux; \
    apt-get update; \
	apt-get install -y --no-install-recommends \
		procps \
        vim \
		mariadb-client \
	; \
	apt-get install -y python3; \
    apt-get install -y python3-pip; \
    pip3 install -r /usr/local/src/requirements.txt -i http://mirrors.aliyun.com/pypi/simple/ --trusted-host mirrors.aliyun.com && \
	rm -rf /var/lib/apt/lists/*

COPY . /tmp/

RUN mkdir -p /opt/azkaban \
 && tar xvf /tmp/azkaban-exec-server-3.84.4.tar.gz -C /opt/azkaban/ \
 && ln -s /opt/azkaban/azkaban-exec-server-3.84.4 /opt/azkaban/azkaban-exec \
 && tar xvf /tmp/azkaban-web-server-3.84.4.tar.gz -C /opt/azkaban/ \
 && ln -s /opt/azkaban/azkaban-web-server-3.84.4 /opt/azkaban/azkaban-web \
 && mv /tmp/start-azkaban.sh /usr/bin \
 && chmod a+x /usr/bin/start-azkaban.sh \
 && mv /tmp/active-exe.sh /usr/bin \
 && chmod a+x /usr/bin/active-exe.sh \
 && rm -rf /tmp/*

WORKDIR /opt/azkaban/

ENV AZKABAN_EXE_HOME=/opt/azkaban/azkaban-exec \
    AZKABAN_WEB_HOME=/opt/azkaban/azkaban-web \
    PATH=${PATH}:${AZKABAN_EXE_HOME}/bin:${AZKABAN_WEB_HOME}/bin

ENTRYPOINT ["start-azkaban.sh"]
CMD [ "exe" ]