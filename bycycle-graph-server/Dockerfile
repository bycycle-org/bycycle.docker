FROM python:3.7-slim

ENV DEBUG true
ENV GRAPH_FILE /project/data/graph.marshal
ENV COST_FUNC bycycle.core.service.route.cost:cost_func
ENV PATH /project/.poetry/bin:$PATH

RUN mv /etc/localtime /etc/localtime.orig
RUN ln -s /usr/share/zoneinfo/America/Los_Angeles /etc/localtime

RUN apt-get update
RUN apt-get upgrade
RUN apt-get -y install curl gcc

RUN adduser --disabled-password --gecos Project --home /project project

USER project

WORKDIR /project

RUN curl -sSL https://raw.githubusercontent.com/sdispater/poetry/master/get-poetry.py | python

RUN python -m venv .

COPY --chown=project graph.marshal /project/data/
COPY --chown=project bycycle.core /project/src/bycycle.core/
COPY --chown=project Dijkstar /project/src/Dijkstar/

RUN . /project/bin/activate && pip install --no-cache-dir --upgrade pip setuptools

WORKDIR /project/src/bycycle.core
RUN . /project/bin/activate && poetry install --no-dev

WORKDIR /project/src/Dijkstar
RUN . /project/bin/activate && pip install --editable .[server]

WORKDIR /project

EXPOSE 8000

CMD ["/project/bin/dijkstar", "serve", "--debug", "--host=0.0.0.0"]
