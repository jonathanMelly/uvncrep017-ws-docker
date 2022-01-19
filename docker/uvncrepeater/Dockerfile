# Parent image di partenza basata su ubuntu/bionic
FROM ubuntu:bionic

# definisco la variabile d'ambiente DEBIAN_FRONTEND
ENV DEBIAN_FRONTEND=noninteractive

# installo tutti i pacchetti necessari allo scaricamento, compilazione e installazione dei sorgenti
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
                    git


# scaricamento del software
CMD ["/bin/bash"]
WORKDIR /usr/local/src
RUN git clone https://github.com/DamianoP/uvncrep017-ws.git
WORKDIR /usr/local/src/uvncrep017-ws/uvncRepeater

# compilazione dei sorgenti e installazione nel sistema
RUN make clean && make \
 && chmod u+x ./install.sh \
 && make install

#creazione dell'utente uvncrep
RUN useradd  -s /sbin/nologin uvncrep

# porte su cui il container resta in ascolto nel mondo "docker", senza esposizione all'esterno
EXPOSE 5500 

# copia dei file necessari per il servizio
WORKDIR /
COPY ./docker/uvncrepeater/entrypoint.sh /usr/bin/uvncrepeater_entrypoint.sh
COPY ./docker/uvncrepeater/uvncrepeater /etc/init.d/uvncrepeater
COPY ./docker/uvncrepeater/uvncrepeater.ini /etc/uvnc/uvncrepeater.ini

# assegnazione corretta dei permessi
CMD ["chmod +x /etc/init.d/uvncrepeater"]
CMD ["chmod 644 /var/log/uvncrepeater.log"]
CMD ["chmod 644 /etc/uvnc/uvncrepeater.ini"]
CMD ["chmod 777 /usr/bin/uvncrepeater_entrypoint.sh"]

# esecuzione dello script di entrypoint il quale avvia il servizio
ENTRYPOINT [ "/bin/sh", "/usr/bin/uvncrepeater_entrypoint.sh" ]
