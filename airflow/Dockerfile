ARG PYTHON_MAJOR_MINOR_VERSION=3.8
ARG PYTHON_BASE_IMAGE=python:3.8-slim-buster
ARG AIRFLOW_VERSION=2.1.0
ARG AIRFLOW_EXTRAS='aws,ssh,sftp,redis,postgres,celery'

FROM apache/airflow:${AIRFLOW_VERSION}-python${PYTHON_MAJOR_MINOR_VERSION}

# envs likely to unchange for a long time
ENV AIRFLOW__CORE__DAGS_ARE_PAUSED_AT_CREATION='true'
ENV AIRFLOW__CORE__EXECUTOR=CeleryExecutor
ENV AIRFLOW__CORE__HOSTNAME_CALLABLE='airflow.utils.net.get_host_ip_address'
ENV AIRFLOW__CORE__LOAD_EXAMPLES='false'
ENV AIRFLOW__CORE__LOAD_DEFAULT_CONNECTIONS='false'
ENV AIRFLOW__CORE__DAGS_ARE_PAUSED_AT_CREATION='true'
ENV AIRFLOW__API__AUTH_BACKEND='airflow.api.auth.backend.basic_auth'
ENV AIRFLOW__CELERY__WORKER_LOG_SERVER_PORT=8793
ENV AIRFLOW__CELERY__WORKER_AUTOSCALE='6,16'

ENV PYTHONPATH=${AIRFLOW_HOME}/

ARG PYTHON_MAJOR_MINOR_VERSION=3.8
ARG CONSTRAINT_FILE_LOCATION=/requirements/constraints-airflow-${AIRFLOW_VERSION}-python-${PYTHON_MAJOR_MINOR_VERSION}.txt

COPY . ${AIRFLOW_HOME}/
COPY ./requirements /requirements
COPY ./scripts /scripts

# apache-airflow recommends not to meddle with dev deps
USER root
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
      build-essential \
      gcc \
      g++ \
      lftp \
      isort \
      htop \
      zip \
      unzip \
      vim \
    && apt-get autoremove -yqq --purge \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Stuff that the pipeline needs to access
USER airflow
RUN pip install -r /requirements/requirements.txt -c ${CONSTRAINT_FILE_LOCATION}
