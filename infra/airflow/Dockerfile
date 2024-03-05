FROM apache/airflow:slim-2.3.2-python3.9
# ENV PYTHONDONTWRITEBYTECODE=1

USER root

RUN apt-get update \
  && apt-get install -y --no-install-recommends \
  build-essential \
  python3-dev \
  default-libmysqlclient-dev \
  pgloader \
  git \
  openjdk-11-jdk \
  && apt-get autoremove -yqq --purge \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

RUN mkdir -p /opt/venv \
  && chown airflow:root /opt/venv

USER airflow

COPY requirements.txt .
RUN pip install --no-cache-dir --user --upgrade pip
RUN pip install --no-cache-dir --user -r requirements.txt

COPY requirements-dbt.txt .
RUN env PIP_USER=0 python3 -m venv /opt/venv/dbt \
  && env PIP_USER=0 /opt/venv/dbt/bin/pip install -r requirements-dbt.txt