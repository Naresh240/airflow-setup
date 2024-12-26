# Airflow setup with postgres database

## Login as root and execute the following commands

```bash
sysctl -w vm.max_map_count=262144
sysctl fs.file-max
ulimit -n
ulimit -u
```

## Setup PostgreSQL15 Database For SonarQube

```bash
yum install postgresql15 postgresql15-server -y
postgresql-setup --initdb
```

Need to change config file as shown in below

```bash
vi /var/lib/pgsql/data/pg_hba.conf
```

Replace Method name "ident" to "md5"

![image](https://user-images.githubusercontent.com/68885738/90953619-aef2f800-e48a-11ea-9b50-489183e9b0c1.png)

Enable  postgresql:

```bash    
systemctl enable postgresql
```
Start postgresql:

```bash
systemctl start postgresql
````

Login into Database

```bash
su - postgres
psql
```

Create a sonarqubedb database, username and provide access to user

```bash
create database airflow_db;
create user airflow_user with encrypted password 'airflow_pass';
grant all privileges on database airflow_db to airflow_user;
\c airflow_db;
GRANT ALL ON SCHEMA public TO airflow_user;
GRANT USAGE ON SCHEMA public TO airflow_user;
\q
exit
```

## Airflow setup with postgres database

Pre-Requisites:

1. Minimum t2.linux server to setup airflow
2. Install python and pip

1. Installation of python and pip

```shell
yum install python python-pip -y
```

2. Setup python virtual environment for airflow

```shell
python -m venv awscli-env
source awscli-env/bin/activate
/root/awscli-env/bin/python -m pip install --upgrade pip
pip install awscli
```

3. Install Airflow using the constraints file, which is determined based on the URL we pass

```shell
AIRFLOW_VERSION=2.10.4
PYTHON_VERSION="$(python -c 'import sys; print(f"{sys.version_info.major}.{sys.version_info.minor}")')"
CONSTRAINT_URL="https://raw.githubusercontent.com/apache/airflow/constraints-${AIRFLOW_VERSION}/constraints-${PYTHON_VERSION}.txt"
pip install "apache-airflow[postgres]==${AIRFLOW_VERSION}" --constraint "${CONSTRAINT_URL}"
```

4. Initialize the Airflow Database

```shell
airflow db init
```

5. Update ```airflow.cfg``` file for postgres database connection and executors

```shell
vi /root/airflow/airflow.cfg
----------------------------
executor = LocalExecutor
sql_alchemy_conn = postgresql+psycopg2://airflow_user:airflow_pass@localhost:5432/airflow_db
```

6. Upgrade Airflow Database with postgres

```shell
airflow db init
```

7. Create Systemd Service for Airflow for webserver and Airflow scheduler

```shell
vi /etc/systemd/system/airflow-webserver.service
---------------------------------------------
[Unit]
Description=Airflow Webserver
After=network.target

[Service]
Environment="AIRFLOW_HOME=/root/airflow"
User=root
Group=root
ExecStart=/root/awscli-env/bin/airflow webserver --port 8080
Restart=always

[Install]
WantedBy=multi-user.target


vi /etc/systemd/system/airflow-scheduler.service
--------------------------------------------------
[Unit]
Description=Airflow Scheduler
After=network.target

[Service]
Environment="AIRFLOW_HOME=/root/airflow"
User=root
Group=root
ExecStart=/root/awscli-env/bin/airflow scheduler
Restart=always

[Install]
WantedBy=multi-user.target
```

8. Run Airflow webserver and scheduler

```shell
service airflow-scheduler start
service airflow-webserver start
```

9. Create user to login airflow

```shell
airflow users create \
    --username naresh240 \
    --firstname naresh \
    --lastname kamani \
    --email naresh240.qis@gmail.com \
    --role Admin \
    --password Naresh#123
```

10. Access Airflow login page

```shell
http://<IP-Address>:<Port>
http://54.196.98.235:8080/
```

![Airflow Login Page](../images/airflow_login.png)
