# Airflow setup

## Pre-Requisites:

1. Minimum t2.linux server to setup airflow
2. Install python and pip

## Installation of python and pip

```shell
yum install python python-pip -y
```

## Setup python virtual environment for airflow

```shell
python -m venv awscli-env
source awscli-env/bin/activate
/root/awscli-env/bin/python -m pip install --upgrade pip
pip install awscli
```

## Install Airflow using the constraints file, which is determined based on the URL we pass

```shell
AIRFLOW_VERSION=2.10.4

# Extract the version of Python you have installed. If you're currently using a Python version that is not supported by Airflow, you may want to set this manually.
# See above for supported versions.
PYTHON_VERSION="$(python -c 'import sys; print(f"{sys.version_info.major}.{sys.version_info.minor}")')"

CONSTRAINT_URL="https://raw.githubusercontent.com/apache/airflow/constraints-${AIRFLOW_VERSION}/constraints-${PYTHON_VERSION}.txt"
# For example this would install 2.10.4 with python 3.8: https://raw.githubusercontent.com/apache/airflow/constraints-2.10.4/constraints-3.8.txt

pip install "apache-airflow==${AIRFLOW_VERSION}" --constraint "${CONSTRAINT_URL}"
```

## Run Airflow Standalone

```shell
airflow standalone
```
