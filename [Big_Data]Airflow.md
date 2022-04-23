### Install airflow   
https://www.udemy.com/course/the-complete-hands-on-course-to-master-apache-airflow/learn/lecture/11999738#overview

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

### Key concept
What should you keep in mind after what you've learned?

Airflow is an orchestrator, not a processing framework, process your gigabytes of data outside of Airflow (i.e. You have a Spark cluster, you use an operator to execute a Spark job, the data is processed in Spark).

A DAG is a data pipeline, an Operator is a task.

An Executor defines how your tasks are execute whereas a worker is a process executing your task

The scheduler schedules your tasks, the web server serves the UI, the database stores the metadata of Airflow.

airflow db init is the first command to execute to initialise Airflow

If a task fails, check the logs by clicking on the task from the UI and "Logs"

The Gantt view is super useful to sport bottlenecks and tasks are too long to execute


### Build a DAG
```
from airflow.models import DAG
from datetime import datetime
from airflow.providers.sqlite.operators.sqlite import SqliteOperator
from airflow.providers.http.sensors.http import HttpSensor
from airflow.providers.http.operators.http import SimpleHttpOperator
from airflow.operators.python import PythonOperator
from airflow.operators.bash import BashOperator
import json
from pandas import json_normalize
default_args = {
    'start_date': datetime(2020, 1, 1)
}
def _processing_user(ti):
    users = ti.xcom_pull(task_ids=['extracting_user'])
    if not len(users) or 'results' not in users[0]:
        raise ValueError('User is empty')
    user = users[0]['results'][0]
    processed_user = json_normalize({
        'firstname': user['name']['first'],
        'lastname': user['name']['last'],
        'country': user['location']['country'],
        'username': user['login']['username'],
        'password': user['login']['password'],
        'email': user['email']
    })
    processed_user.to_csv('/tmp/processed_user.csv', index=None, header=False)

with DAG(
    'user_processing',  #dag id
    schedule_interval='@daily', 
    default_args=default_args, 
    catchup=False) as dag:
    # Define tasks/operators
    creating_table = SqliteOperator(
        task_id = 'creating_table',
        sqlite_conn_id='db_sqlite',
        sql='''
            CREATE TABLE IF NOT EXISTSusers(
                firstname TEXT NOT NULL,
                lastname TEXT NOT NULL,
                country TEXT NOT NULL,
                username TEXT NOT NULL,
                password TEXT NOT NULL, 
                email TEXT NOT NULL PRIMARY KEY
            );
        '''
    )

    is_api_available = HttpSensor(
        task_id='is_api_available',
        http_conn_id='user_api',
        endpoint='api/'
    )

    extracting_user = SimpleHttpOperator(
        task_id='extracting_user',
        http_conn_id='user_api',
        endpoint = 'api/',
        method='GET',
        response_filter=lambda response: json.loads(response.text),
        log_response=True
    )


    processing_user = PythonOperator(
        task_id='processing_user',
        python_callable=_processing_user
    )

    storing_user = BashOperator(
        task_id='storing_user',
        bash_command='echo -e ".separator ","\n.import /tmp/processed_user.csv users" | sqlite3 /home/airflow/airflow/airflow.db'
    )

    creating_table >> is_api_available >> extracting_user >> processing_user >> storing_user
  

```
