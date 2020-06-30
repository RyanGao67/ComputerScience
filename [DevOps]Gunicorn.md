
### Installation:

```
pip install gunicorn
```
Assumption: python 3.0+
```
pip install flask

pip install flask-restful

$which gunicorn
```
Deploy the flask API with gunicorn 
```
$gunicorn test:app
```
**Important: For flask app with factory pattern, user need to  create a new script(e.g. wsgi.py), then deploy the create_app() method, instead run the created app in the script.**  
[https://stackoverflow.com/questions/25319690/how-do-i-run-a-flask-app-in-gunicorn-if-i-used-the-application-factory-pattern/25319752](https://stackoverflow.com/questions/25319690/how-do-i-run-a-flask-app-in-gunicorn-if-i-used-the-application-factory-pattern/25319752)

Create python script to import create method:   

```
from my_project import create_app
app = create_app()
```

Then use gunicorn to deploy the flask app:

```
gunicorn my_project:create_app()
```


Result:

Test script:

In command line 

$gunicorn test:app
[2020-02-12 14:35:26 -0500] [28288] [INFO] Starting gunicorn 20.0.4
[2020-02-12 14:35:26 -0500] [28288] [INFO] Listening at: http://127.0.0.1:8000 (28288)
[2020-02-12 14:35:26 -0500] [28288] [INFO] Using worker: sync
[2020-02-12 14:35:26 -0500] [28291] [INFO] Booting worker with pid: 28291


Open http://127.0.0.1:8000 on web

Config parameters:

Simple set up:

Use command line options: the example set up gunicorn with 4 workers 
```
$gunicorn -w 4 test:app
```
Set up command arguments: the example set up gunicorn with 3 workers and 127.0.0.1 as host 
```
$ GUNICORN_CMD_ARGS="--bind=127.0.0.1 --workers=3" gunicorn test:app
```
Use config file:
The config file must have a python extension

To set a parameter, just assign to it. There’s no special syntax.

Config example: 

Using config: gunicorn_config.py
```
# gunicorn_config.py

workers = 4
threads = 2
bind = '127.0.0.1:5000'
daemon = 'false'
worker_connections = 5
accesslog = 'access.log'
errorlog = 'error.log'
loglevel = 'info'
```
Then in command line: 
```
$gunicorn -c gunicorn_config.py test:app
```
Available parameters are listed at the end of this page.

Parameter tuning:

workers: gunicorn should only need 4-12 worker processes, recommended number is (2 * CPU) + 1

threads: depending on system, using multiple threads, multiple workers or some mixture may yield best results.

The suggested maximum concurrent requests when using workers and threads is still (2 * CPU) + 1

Concurrent = num_workers * num_threads

For example, for a 4-core computer:

maximum concurrent = 2 * 4 +1 =9

So we need a combination of workers and threads that production equals 9, such as 3 workers and 3 threads: 
```
$gunicorn --workers=3 --threads=3 test:app
```
Possible parameters:
```
(test1) jzhang@PC:~/Desktop$ gunicorn -h
usage: gunicorn [OPTIONS] [APP_MODULE]

optional arguments:
  -h, --help            show this help message and exit
  -v, --version         show program's version number and exit
  -c CONFIG, --config CONFIG
                        The Gunicorn config file. [None]
  -b ADDRESS, --bind ADDRESS
                        The socket to bind. [['127.0.0.1:8000']]
  --backlog INT         The maximum number of pending connections. [2048]
  -w INT, --workers INT
                        The number of worker processes for handling requests.
                        [1]
  -k STRING, --worker-class STRING
                        The type of workers to use. [sync]
  --threads INT         The number of worker threads for handling requests.
                        [1]
  --worker-connections INT
                        The maximum number of simultaneous clients. [1000]
  --max-requests INT    The maximum number of requests a worker will process
                        before restarting. [0]
  --max-requests-jitter INT
                        The maximum jitter to add to the *max_requests*
                        setting. [0]
  -t INT, --timeout INT
                        Workers silent for more than this many seconds are
                        killed and restarted. [30]
  --graceful-timeout INT
                        Timeout for graceful workers restart. [30]
  --keep-alive INT      The number of seconds to wait for requests on a Keep-
                        Alive connection. [2]
  --limit-request-line INT
                        The maximum size of HTTP request line in bytes. [4094]
  --limit-request-fields INT
                        Limit the number of HTTP headers fields in a request.
                        [100]
  --limit-request-field_size INT
                        Limit the allowed size of an HTTP request header
                        field. [8190]
  --reload              Restart workers when code changes. [False]
  --reload-engine STRING
                        The implementation that should be used to power
                        :ref:`reload`. [auto]
  --reload-extra-file FILES
                        Extends :ref:`reload` option to also watch and reload
                        on additional files [[]]
  --spew                Install a trace function that spews every line
                        executed by the server. [False]
  --check-config        Check the configuration. [False]
  --preload             Load application code before the worker processes are
                        forked. [False]
  --no-sendfile         Disables the use of ``sendfile()``. [None]
  --reuse-port          Set the ``SO_REUSEPORT`` flag on the listening socket.
                        [False]
  --chdir CHDIR         Chdir to specified directory before apps loading.
                        [/home/jzhang/Desktop]
  -D, --daemon          Daemonize the Gunicorn process. [False]
  -e ENV, --env ENV     Set environment variable (key=value). [[]]
  -p FILE, --pid FILE   A filename to use for the PID file. [None]
  --worker-tmp-dir DIR  A directory to use for the worker heartbeat temporary
                        file. [None]
  -u USER, --user USER  Switch worker processes to run as this user. [1000]
  -g GROUP, --group GROUP
                        Switch worker process to run as this group. [1000]
  -m INT, --umask INT   A bit mask for the file mode on files written by
                        Gunicorn. [0]
  --initgroups          If true, set the worker process's group access list
                        with all of the [False]
  --forwarded-allow-ips STRING
                        Front-end's IPs from which allowed to handle set
                        secure headers. [127.0.0.1]
  --access-logfile FILE
                        The Access log file to write to. [None]
  --disable-redirect-access-to-syslog
                        Disable redirect access logs to syslog. [False]
  --access-logformat STRING
                        The access log format. [%(h)s %(l)s %(u)s %(t)s
                        "%(r)s" %(s)s %(b)s "%(f)s" "%(a)s"]
  --error-logfile FILE, --log-file FILE
                        The Error log file to write to. [-]
  --log-level LEVEL     The granularity of Error log outputs. [info]
  --capture-output      Redirect stdout/stderr to specified file in
                        :ref:`errorlog`. [False]
  --logger-class STRING
                        The logger you want to use to log events in Gunicorn.
                        [gunicorn.glogging.Logger]
  --log-config FILE     The log config file to use. [None]
  --log-config-dict LOGCONFIG_DICT
                        The log config dictionary to use, using the standard
                        Python [{}]
  --log-syslog-to SYSLOG_ADDR
                        Address to send syslog messages. [udp://localhost:514]
  --log-syslog          Send *Gunicorn* logs to syslog. [False]
  --log-syslog-prefix SYSLOG_PREFIX
                        Makes Gunicorn use the parameter as program-name in
                        the syslog entries. [None]
  --log-syslog-facility SYSLOG_FACILITY
                        Syslog facility name [user]
  -R, --enable-stdio-inheritance
                        Enable stdio inheritance. [False]
  --statsd-host STATSD_ADDR
                        ``host:port`` of the statsd server to log to. [None]
  --dogstatsd-tags DOGSTATSD_TAGS
                        A comma-delimited list of datadog statsd (dogstatsd)
                        tags to append to statsd metrics. []
  --statsd-prefix STATSD_PREFIX
                        Prefix to use when emitting statsd metrics (a trailing
                        ``.`` is added, []
  -n STRING, --name STRING
                        A base to use with setproctitle for process naming.
                        [None]
  --pythonpath STRING   A comma-separated list of directories to add to the
                        Python path. [None]
  --paste STRING, --paster STRING
                        Load a PasteDeploy config file. The argument may
                        contain a ``#`` [None]
  --proxy-protocol      Enable detect PROXY protocol (PROXY mode). [False]
  --proxy-allow-from PROXY_ALLOW_IPS
                        Front-end's IPs from which allowed accept proxy
                        requests (comma separate). [127.0.0.1]
  --keyfile FILE        SSL key file [None]
  --certfile FILE       SSL certificate file [None]
  --ssl-version SSL_VERSION
                        SSL version to use. [_SSLMethod.PROTOCOL_TLS]
  --cert-reqs CERT_REQS
                        Whether client certificate is required (see stdlib ssl
                        module's) [VerifyMode.CERT_NONE]
  --ca-certs FILE       CA certificates file [None]
  --suppress-ragged-eofs
                        Suppress ragged EOFs (see stdlib ssl module's) [True]
  --do-handshake-on-connect
                        Whether to perform SSL handshake on socket connect
                        (see stdlib ssl module's) [False]
  --ciphers CIPHERS     SSL Cipher suite to use, in the format of an OpenSSL
                        cipher list. [None]
  --paste-global CONF   Set a PasteDeploy global config variable in
                        ``key=value`` form. [[]]
  --strip-header-spaces
                        Strip spaces present between the header name and the
                        the ``:``. [False]
                        
                        ```
                        
                        
                        
                        
                        
                        
                        
                        
                        
                        
GIT CI FILE

deploy_stage:
  stage: deploy
  tags:
    - deployement
  script:
    - export SSHPASS=$PASSA
    - cd ~/ && rm -rf portal_muhc; rm -rf portal_2
    - cd ~/ && git clone -b dev https://mamanuddin:$PASSG@git.indocresearch.org/aws_muhc/portal_muhc.git
    - cd ~/portal_muhc && mv server muhc_backend
    - sshpass -e scp -o stricthostkeychecking=no -r muhc_backend ubuntu@35.182.112.167:/home/ubuntu/
    - sshpass -e ssh -o stricthostkeychecking=no ubuntu@35.182.112.167 'cd /home/ubuntu/muhc_backend/ && echo $PASSA | sudo -S service gunicorn restart'
    
   
    - cd ~/ && mv portal_muhc portal_2
    - sshpass -e scp -o stricthostkeychecking=no -r portal_2 ubuntu@35.183.166.207:/home/ubuntu/
    - sshpass -e ssh -o stricthostkeychecking=no ubuntu@35.183.166.207 'cd /home/ubuntu/portal_2/ &&  npm install; rm -rf .git; pm2 kill; pm2 start /home/ubuntu/portal_2/node_modules/react-app-rewired/scripts/start.js;'
    - cd ~/ && rm -rf portal_2

 

Systemd file for gunicorn

The path the file is /etc/systemd/system/gunicorn.service

 

[Unit]
Description=AmaanExample
After=network.target

[Service]
Type=simple
WorkingDirectory=/home/ubuntu/muhc_backend/
User=ubuntu
ExecStart= /home/ubuntu/anaconda3/bin/gunicorn  -c gunicorn_config.py wsgi:app

[Install]
WantedBy=multi-user.target
 

After creating the file run sudo systemctl enable gunicorn
