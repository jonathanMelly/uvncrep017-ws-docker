FROM ubuntu:bionic

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update --yes \
 && apt-get install --no-install-recommends --yes \
                    build-essential \
                    gzip \
                    libx11-6 \
                    libx11-dev \
                    libxtst6 \
                    nano \
                    psmisc \
                    rsync \
                    x-window-system-core \
                    x-window-system \
                    wget \
                    git

CMD ["/bin/bash"]

WORKDIR /usr/local/src
RUN git clone https://github.com/DamianoP/uvncrep017-ws.git
WORKDIR /usr/local/src/uvncrep017-ws/uvncRepeater

RUN make clean && make \
 && chmod u+x ./install.sh \
 && make install

RUN useradd  -s /sbin/nologin uvncrep
EXPOSE 5500 5901


RUN ["chmod", "+x", "/etc/init.d/uvncrepeater"]

CMD ["/bin/bash"]
WORKDIR /

COPY ./docker/uvncrepeater/entrypoint.sh /usr/bin/uvncrepeater_entrypoint.sh
COPY ./docker/uvncrepeater/uvncrepeater /etc/init.d/uvncrepeater
COPY ./docker/uvncrepeater/uvncrepeater.ini /etc/uvnc/uvncrepeater.ini

CMD ["chmod 644 /var/log/uvncrepeater.log"]
CMD ["chmod 644 /etc/uvnc/uvncrepeater.ini"]

RUN chmod 777 /usr/bin/uvncrepeater_entrypoint.sh
ENTRYPOINT ["/usr/bin/uvncrepeater_entrypoint.sh"]
