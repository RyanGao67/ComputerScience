### Install airflow

### Create Users
```
 airflow users create -u admin -p admin -f tian -l gao -r Admin -e admin@airflow.com

```

### Useful command

```
#initiate the metadata store of airflow, as well as generating the files and folders needed by airflow. You will execute that command only once and you shouldn't use it anymore.
airflow db init

#upgrade your information from 1.10 to 1.12, etc
airflow do upgrade

# everything in airflow db will lost
airflow db reset

# start user interface of airflow
airflow webserver

# start scheduler, scheduler in charge of scheduling your data pipelines and so your tasks
airflow scheduler

# list all the dags
airflow dags list

# know the tasks the dags have
airflow tasks list example_xcom_args

# trigger a dag at 2020-01-01
airflow dags trigger -e 2020-01-01 example_xcom_args
```
