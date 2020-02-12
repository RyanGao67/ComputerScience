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
# two things here 
# 'tasks' the name for the list of tasks that follow
# broker are where the messages are going to be stored
app = Celery('tasks', broker='amqp://localhost//')
@app.task
def reverse(string):
  return string[::-1]
```
