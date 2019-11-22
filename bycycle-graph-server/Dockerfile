FROM python:3.7-slim

ENV DEBUG true
ENV GRAPH_FILE /project/data/graph.marshal
ENV COST_FUNC dijkstar_cost:cost_func

RUN mv /etc/localtime /etc/localtime.orig
RUN ln -s /usr/share/zoneinfo/America/Los_Angeles /etc/localtime

RUN apt-get -y update
RUN apt-get -y upgrade
RUN apt-get -y install gcc
RUN rm -rf /var/lib/apt/lists/*

RUN adduser --disabled-password --gecos Project --home /project project

USER project

RUN python -m venv /project
RUN . /project/bin/activate && pip install --no-cache-dir --no-compile --upgrade pip setuptools
RUN . /project/bin/activate && pip install --no-compile Dijkstar[server]==3.0a4

COPY --chown=project bycycle.core/bycycle/core/service/route/cost.py /project/lib/python3.7/site-packages/dijkstar_cost.py
COPY --chown=project graph.marshal /project/data/

WORKDIR /project

EXPOSE 8000

CMD ["/project/bin/dijkstar", "serve", "--debug", "--host=0.0.0.0"]
