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

```Note:``` 
1. Please check directory and file permissions using below commands
2. Here modules need to add under ```requirements.txt``` file and then restart airflow

```shell
chmod 777 dags
chmod 777 plugins
chmod 777 logs
chmod 777 config
```

## Login to Airflow

```shell
# Default credentials
username: airflow
password: airflow
```

![Airflow Login Page](../images/airflow_login.png)

## Checking mlops pipeline under airflow

![MLOPS pipeline Demo](../images/ml_pipeline_demo.png)

After running dag:

![MLOPS pipeline Demo RUN](../images/ml_pipeline_demo_run.png)
