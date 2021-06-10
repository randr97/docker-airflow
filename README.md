# Airflow setup

> Note: The docker-compose file is ref from apache airflow official docker-compose and is edited

This setup is a replica of official apache-airflow docker setup

## General instructions
1. To add airflow conn using env use `AIRFLOW_CONN_<ur-conn-name>`
2. To modify .cfg file of airflow then use env `AIRFLOW_<CFG-GROUP>_<SETTING>`

## Local setup steps
1. Run command docker-compose up airflow-init
2. The build will happen ref the Docker file present in the folder
3. `dags` folder is mounted to `${AIRFLOW_HOME}/dags` and similarly the `plugins`
4. The airflow env values in docker-compose should not be taken at face value for prod
5. Default user name and pass for local is `airflow` - ref docker-compose file for this

## Prod points to be noted
1. `_AIRFLOW_WWW_USER_CREATE` .. `_AIRFLOW_WWW_USER_USERNAME` .. `_AIRFLOW_WWW_USER_PASSWORD`
should be strictly avoided
2. How to create admin user?
```
airflow users create \
--username admin \
--firstname Peter \
--lastname Parker \
--role Admin \
--email rohit@admin.org
```
3. Very important - `requirements.txt` is constrained by `constraints-airflow-2.1.0-python-3.8.txt`
