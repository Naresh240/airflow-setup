# Airflow With Docker Compose

## Pre-Requisites:
- Install docker
- Install docker-compose

## Install docker 

```shell
yum install docker -y
service docker start
service docker status
```

## Install docker-compose

```shell
curl -SL https://github.com/docker/compose/releases/download/v2.32.0/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
docker-compose version
```

## Create directories with below command

```shell
mkdir -p ./dags ./logs ./plugins ./config
echo -e "AIRFLOW_UID=$(id -u)" > .env
```

For other operating systems, you may get a warning that AIRFLOW_UID is not set, but you can safely ignore it. You can also manually create an .env file in the same folder as docker-compose.yaml with this content to get rid of the warning:

```shell
AIRFLOW_UID=50000
```

## Setup Airflow with postgres using docker-compose

```shell
docker-compose up -d
```

```Note:``` Please check directory and file permissions using below commands

```shell
chmod 777 dags
chmod 777 plugins
chmod 777 logs
chmod 777 config
```
