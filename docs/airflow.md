
## What is Airflow?
Airflow is a platform to programmatically author, schedule and monitor workflows or data pipelines.

## What is a Workflow?
* a sequence of tasks
* started on a schedule or triggered by an event
* frequently used to handle big data processing pipelines

**A typical workflows**

1. download data from source
2. send data somewhere else to process
3. Monitor when the process is completed
4. Get the result and generate the report
5. Send the report out by email



### Prerequisites
Airflow is written in Python, so I will assume you have it installed on your machine. I’m using Python 3 (because it’s 2017, come on people!), but Airflow is supported on Python 2 as well. I will also assume that you have virtualenv installed.

`python3 --version`

    Python 3.6.0
`virtualenv --version`

    15.1.0

### Install Airflow

Let’s create a workspace directory, and inside it a Python 3 virtualenv directory:

```
cd /path/to/my/airflow/workspace
virtualenv -p `which python3` venv
    or
virtualenv -p python3 myenv
source venv/bin/activate\
```

Now let’s install Airflow 1.8:

    (venv) $ pip install apache-airflow

**If ERROR :**

Error while install airflow: By default one of Airflow's dependencies installs a GPL
raise RuntimeError("By default one of Airflow's dependencies installs a GPL "

    RuntimeError: By default one of Airflow's dependencies installs a GPL dependency (unidecode). To avoid this dependency set SLUGIFY_USES_TEXT_UNIDECODE=yes in your environment when you install or upgrade Airflow. To force installing the GPL version set AIRFLOW_GPL_UNIDECODE

`export AIRFLOW_GPL_UNIDECODE=yes`
or
`export SLUGIFY_USES_TEXT_UNIDECODE=yes`    

Using export makes the environment variable available to all the subprocesses.
Also, make sure you are using pip install apache-airflow



Now we’ll need to create the AIRFLOW_HOME directory where your DAG definition files and Airflow plugins will be stored. Once the directory is created, set the AIRFLOW_HOME environment variable:

`
(venv) $ cd /path/to/my/airflow/workspace
(venv) $ mkdir airflow_home
(venv) $ export AIRFLOW_HOME='pwd'/airflow_home
(venv) $ airflow initdb
`

You should now be able to run Airflow commands. Let’s try by issuing the following:

`(venv) $ airflow version`

```buildoutcfg 
     ____________       _____________
 ____    |__( )_________  __/__  /________      __
____  /| |_  /__  ___/_  /_ __  /_  __ \_ | /| / /
___  ___ |  / _  /   _  __/ _  / / /_/ /_ |/ |/ /
 _/_/  |_/_/  /_/    /_/    /_/  \____/____/|__/
   v1.8.0rc5+apache.incubating
```

If the airflow version command worked, then Airflow also created its default configuration file airflow.cfg in AIRFLOW_HOME:

```
airflow_home
├── airflow.cfg
└── unittests.cfg
```
Default configuration values stored in airflow.cfg will be fine for this tutorial, but in case you want to tweak any Airflow settings, this is the file to change.

Initialize the Airflow DB

Next step is to issue the following command, which will create and initialize the Airflow SQLite database:

    (venv) $ airflow initdb

The database will be created in airflow.db by default.

```
airflow_home
├── airflow.cfg
├── airflow.db        <- Airflow SQLite DB
└── unittests.cfg
```

Using SQLite is an adequate solution for local testing and development, but it does not support concurrent access. In a production environment, you will most certainly want to use a more robust database solution such as Postgres or MySQL.

Start the Airflow web server

Airflow’s UI is provided in the form of a Flask web application. You can start it by issuing the command:

    (venv) $ airflow webserver

Now visit the Airflow UI by navigating your browser to port 8080 on the host where Airflow was started, for example: http://localhost:8080/admin/

Airflow comes with a number of example DAGs. Note that these examples may not work until you have at least one DAG definition file in your own dags_folder. You can hide the example DAGs by changing the load_examples setting in airflow.cfg.

First Airflow DAG

OK, if everything is ready, let’s start writing some code. We’ll start by creating a Hello World workflow, which does nothing other then sending “Hello world!” to the log.
Create your dags_folder, that is the directory where your DAG definition files will be stored in AIRFLOW_HOME/dags. Inside that directory create a file named hello_world.py.

```
airflow_home
├── airflow.cfg
├── airflow.db
├── dags                <- Your DAGs directory
│   └── hello_world.py  <- Your DAG definition file
└── unittests.cfg
```

Add the following code to dags/hello_world.py:
airflow_home/dags/hello_world.pyThis file creates a simple DAG with just two operators, the DummyOperator, which does nothing and a PythonOperator which calls the print_hello function when its task is executed.

Running the DAG
In order to run DAG, open a second terminal and start the Airflow scheduler by issuing the following commands:

```
from datetime import datetime
from airflow import DAG
from airflow.operators.dummy_operator import DummyOperator
from airflow.operators.python_operator import PythonOperator

def print_hello():
    return 'Hello world!'

dag = DAG('hello_world', description='Simple tutorial DAG',
          schedule_interval='0 12 * * *',
          start_date=datetime(2017, 3, 20), catchup=False)

dummy_operator = DummyOperator(task_id='dummy_task', retries=3, dag=dag)

hello_operator = PythonOperator(task_id='hello_task', python_callable=print_hello, dag=dag)

dummy_operator >> hello_operator
```



    $ cd /path/to/my/airflow/workspace
    $ export AIRFLOW_HOME=`pwd`/airflow_home
    $ source venv/bin/activate
    (venv) $ airflow scheduler        



The scheduler will send tasks for execution. The default Airflow settings rely on an executor named SequentialExecutor, which is started automatically by the scheduler. In production you would probably want to use a more robust executor, such as the CeleryExecutor.
When you reload the Airflow UI in your browser, you should see your hello_world DAG listed in Airflow UI.

Hello World DAG in Airflow UI
In order to start a DAG Run, first turn the workflow on (arrow 1), then click the Trigger Dagbutton (arrow 2) and finally, click on the Graph View (arrow 3) to see the progress of the run.

Hello World DAG Run - Graph View
You can reload the graph view until both tasks reach the status Success. When they are done, you can click on the hello_task and then click View Log. If everything worked as expected, the log should show a number of lines
