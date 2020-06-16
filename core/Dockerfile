FROM centos:7
WORKDIR /opt
ARG Version=1.5.9
ENV Version=${Version} \
    LANG=en_US.utf8

RUN set -ex \
    && yum install -y epel-release wget \
    && ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "LANG=en_US.utf8" > /etc/locale.conf \
    && yum -y install gcc nc git \
    && yum -y install python36 python36-devel unzip openldap-devel \
    && git clone --depth=1 https://github.com/Suvigo/jumpserver.git \
    && chown -R root:root jumpserver \
    && yum -y install $(cat /opt/jumpserver/requirements/rpm_requirements.txt) \
    && python3.6 -m venv /opt/py3 \
    && source /opt/py3/bin/activate \
    && pip install wheel \
    && pip install --upgrade pip setuptools \
    && pip install six==1.5.0 -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com \
    && pip install pyasn1==0.1.2 -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com \
    && pip install pbr==2.0.0 -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com \
    && pip install -r /opt/jumpserver/requirements/requirements.txt -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com \  
    && yum remove -y wget gcc unzip \
    && yum clean all \
    && rm -rf /var/cache/yum/* \
    && rm -rf /opt/${Version}.zip \
    && rm -rf /var/cache/yum* \
    && rm -rf ~/.cache/pip

COPY core/entrypoint.sh .
RUN chmod 755 ./entrypoint.sh

CMD ["./entrypoint.sh"]
