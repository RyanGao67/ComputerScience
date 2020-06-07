### In this Example, I will discribe the following use case:
* User upload a file to a folder(can be viewed as a data lake)
* When the file landed in this folder, a event is triggered(eg, file created, read a file, finish writing to a file)
* The event will be captured, and necessary information will be sent to a queue which is connected to a worker.
* The worker will extract the information from the file(in this case, csv, tsv, xls) and organize the information to send to database(postgres or elasticsearch)

### Needed tools
* NFS(just a file sharing protocal, no need to dig deep)
* Python3
* pyinotify
* redis(you can use rabbitmq, etc)
* celery
* elasticsearch
* pandas

### Set up the environment
* Install anaconda, create the environment, install python3, pyinotify, redis, celery, elasticsearch, pandas into python env
* Install redis to your machine
[https://www.youtube.com/watch?v=Hbt56gFj998&t=71s](https://www.youtube.com/watch?v=Hbt56gFj998&t=71s)
* Install celery to your machine
[https://www.youtube.com/watch?v=fg-JfZBetpM&list=PLXmMXHVSvS-DvYrjHcZOg7262I9sGBLFR&index=2&t=13s](https://www.youtube.com/watch?v=fg-JfZBetpM&list=PLXmMXHVSvS-DvYrjHcZOg7262I9sGBLFR&index=2&t=13s)

### The worker job   
```python
import os
import pandas as pd
import csv

from celery import Celery
from collections import OrderedDict
from elasticsearch import Elasticsearch

app = Celery('tasks', broker='redis://localhost:6379/0')

def connect_elasticsearch():
    _es = None
    _es = Elasticsearch([{'host': '10.1.1.103', 'port': 9201}])
    if _es.ping():
        print('Yay Connect')
    else:
        print('Awww it could not connect!')
    return _es
es = connect_elasticsearch()


# folder = "/mnt/nfs/indoc-labkey-fileroot/PERFCT/PERFCT_V2/@files/ION_TORRENT_DATA_UPLOAD"

def handle_csv_elas(new_file_name, current_patient, patient_id):
    count = 0
    titles = []
    with open(new_file_name, "r") as f:
        read = csv.reader(f)
        for row in read :
            if count==0:
                titles = row
            else:
                write_to_elas = {}
                locus = ''
                genotype = ''
                for i in range(len(titles)):
                    if titles[i] == 'Locus':
                        locus = row[i]
                    if titles[i] == 'Genotype':
                        genotype = row[i]
                    write_to_elas[titles[i]] = row[i]

                write_to_elas['current_patient'] = current_patient
                write_to_elas['participant_id'] = patient_id

                res = es.index(index="perfct",  id=current_patient+locus+genotype,body=write_to_elas)

            count+=1

    os.remove(new_file_name)


@app.task
def etl(file_path):
    filename, extension = file_path.split("/")[-1].split(".")
    print(filename, extension)
    if extension=="tsv":
        csv_table=pd.read_table(file_path,encoding = "utf-8", sep='\t')
        new_file_name = filename+'.'+'csv'
        csv_table.to_csv(new_file_name,index=False)
        current_patient = ''
        patient_id = ''


        with open(new_file_name, "r") as f:
            lines = f.readlines()
        with open(new_file_name, "w") as f:
            for line in lines:
                current_line = line.strip("\n").lstrip('"')
                file_info = current_line.split(',')[0]
                if len(file_info.split("="))==2:
                    patientKey, patientValue = file_info.split("=")
                else :
                    patientKey, patientValue ="",""

                if patientKey=="##sampleNames":
                    current_patient=patientValue
                    patient_id,_=current_patient.split('_')
                    patient_id = patient_id[7:11]

                if not current_line.startswith("##"):
                    f.write(line)

        handle_csv_elas(new_file_name, current_patient, patient_id)

```
### The file watchdog
```python

import pyinotify
from event_trigger import etl

class MyEventHandler(pyinotify.ProcessEvent):
#    def process_IN_ACCESS(self, event):
#        print("ACCESS event:", event.pathname)

#    def process_IN_ATTRIB(self, event):
#        print("ATTRIB event:", event.pathname)

    def process_IN_CLOSE_NOWRITE(self, event):
        print("CLOSE_NOWRITE event:", event.pathname)

    def process_IN_CLOSE_WRITE(self, event):
        print("CLOSE_WRITE event:", event.pathname)
        etl.delay(event.pathname)

    def process_IN_CREATE(self, event):
        print("CREATE event:", event.pathname)

#    def process_IN_DELETE(self, event):
#        print("DELETE event:", event.pathname)

#    def process_IN_MODIFY(self, event):
#        print("MODIFY event:", event.pathname)

#    def process_IN_OPEN(self, event):
#        print("OPEN event:", event.pathname)

def main():
    # watch manager
    wm = pyinotify.WatchManager()
    wm.add_watch('/home/in/perfct_phase2', pyinotify.ALL_EVENTS, rec=True, auto_add=True)

    # event handler
    eh = MyEventHandler()

    # notifier
    notifier = pyinotify.Notifier(wm, eh)
    notifier.loop()

if __name__ == '__main__':
    main()

```

### How to run the service
First start redis, Then start celery
```
celery -A event_trigger worker --loglevel=info
```
Run the file watch dog
