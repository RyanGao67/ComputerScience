# Step 1:
sudo apt-get install rabbitmq-server

Step 2: 
sudo service rabbitmq-server restart

Step 3:
sudo rabbitmqctl status

Step 4:
sudo pip install celery

Step 5:



```python
from celery import Celery
import time
from flask import Flask
from flask_sqlalchemy import SQLAlchemy
from random import choice

# app = Celery(
#     'tasks', 
#     broker='amqp://localhost//', 
#     backend="db+postgresql://testuser:tgao@localhost:5432/testdb")

# @app.task
# def reverse(string):
#     time.sleep(1000000)
#     return string[::-1]


def make_celery(app):
    celery = Celery(app.import_name, backend=app.config['CELERY_BACKEND'],
                    broker=app.config['CELERY_BROKER_URL'])
    celery.conf.update(app.config)
    TaskBase = celery.Task
    class ContextTask(TaskBase):
        abstract = True
        def __call__(self, *args, **kwargs):
            with app.app_context():
                return TaskBase.__call__(self, *args, **kwargs)
    celery.Task = ContextTask
    return celery

myapp = Flask(__name__)
myapp.config['CELERY_BROKER_URL'] = "amqp://localhost//"
myapp.config['CELERY_BACKEND'] = "db+postgresql://testuser:tgao@localhost:5432/testdb"
myapp.config['SQLALCHEMY_DATABASE_URI'] = "postgresql://testuser:tgao@localhost:5432/testdb"
celery = make_celery(myapp)
db = SQLAlchemy(myapp)

@myapp.route('/process/<name>')
def process(name):
    reverse.delay(name)
    return 'I sent an async request!'

@celery.task(name='celery_exampel.reverse')
def reverse(string):
    return string[::-1]

@myapp.route('/insertData')
def insertData():
    insert.delay()
    return "sent to celry"

class Results(db.Model):
    id = db.Column('id', db.Integer, primary_key=True)
    data = db.Column('data', db.String(50))

@celery.task(name='celery_example.insert')
def insert():
    for i in range(50000):
         data = ''.join(choice('ABCDE') for i in range(10))
         result = Results(data=data)
         db.session.add(result)
    db.session.commit()
    return "Done"

if __name__=="__main__":
    myapp.run(debug=True)
```

Step 6: running celery 
// flask_celery is the flask_celery.py flask_celery.celery specify the celery instance in this py
```
celery -A flask_celery.celery worker --loglevel=info
```

Step 7: start app:
python flask_celery.py


