FROM postgres:13-bullseye

RUN apt-get -y update
RUN apt-get -y upgrade
RUN apt-get -y install postgresql-13-postgis-3
RUN rm -rf /var/lib/apt/lists/*
