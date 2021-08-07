## Basic Airflow in Python

Airflow: platform to program workflows.  

DAG: workflow made up of tasks with dependencies.  

Define a DAG in Python:
```
from airflow.models import DAG
from datetime import datetime

#create a default arguments dictionary (optional)
default_arguments = {
     'owner': 'massy',
     'startdate': datetime(2021,8,4)     #earliest datetime DAG could be run
}

#define the DAG object
etl_dag = DAG('etl_workflow', default_args = default_arguments)
``` 

The Airflow configurations and settings are in the airflow.cfg file.  

<br>

### Operators

Most common Airflow task: Operator.  

**Bash Operator**  

```
from airflow.operators.bash_operator import BashOperator

example_task = BashOperator(
     task_id='example',
     bash_command='example.sh',
     dag=etl_dag)
``` 


**Python Operator**  

```
from airflow.operators.python_operator import PythonOperator

def printme():
     print("well done!")

python_task = PythonOperator(
     task_id='print',
     #call the Python function
     python_callable=printme,
     dag=etl_dag)
``` 
To implement keyword arguments with the Python Operator, we define an argument on the task called *op_kwargs*. This is a dictionary consisting of the named arguments for the intended Python function.  
Example of a task to download and save a file to the system within Airflow:

```
def pull_file(URL, savepath):
    r = requests.get(URL)
    with open(savepath, 'wb') as f:
        f.write(r.content)   
    #print method for logging
    print(f"File pulled from {URL} and saved to {savepath}")

from airflow.operators.python_operator import PythonOperator

#Create the task
pull_file_task = PythonOperator(
    task_id='pull_file',
    python_callable=pull_file,
    #Define the arguments
    op_kwargs={'URL':'http://dataserver/sales.json', 'savepath':'latestsales.json'},
    dag=etl_dag
)
``` 


**Email Operator**  

```
from airflow.operators.email_operator import EmailOperator

email_task = EmailOperator(
     task_id='Send_Email',
     to='manager@example.com',
     subject='Automated Daily Report',
     html_content='Attached is the daily report',
     files='report.xlsx',
     dag=etl_dag)
```

**Sensor**  

Special type of operator that waits for a certain condition (ex. creation of a file, upload a database record, a response from a web request) to be true. We define how often to check for the condition to be true.  
Sensor are assigned to tasks like normal operators.  
Arguments:  
* *mode* -> how to check for the condition  
 * mode='poke' -> default, run repeatedly  
 * mode='reschedule' -> give up task slot and try again later  
* *poke_interval* -> how often to wait between checks  
* *timeout* -> how long to wait before failing task  

Ex. File Sensor (check for the existence of a file in a location):

```
from airflow.contrib.sensors.file_sensor import FileSensor

file_sensor_task = FileSensor(task_id='file_pres',                              
     filepath='data.csv',
     poke_interval=300,
     dag=etl_dag)
``` 
Other sensors:  
* *ExternalTaskSensor* -> wait for a task in another DAG to complete  
* *HttpSensor* -> request a web URL and check for content  
* *SqlSensor* -> run a SQL query to check for content  




<br>

### Dependencies

Task dependencies define a given order of task completion and are referred to *upstream* (>>, before) or *downstream* (<<, after) tasks.

```
task1 = BashOperator(
     task_id='first',
     bash_command='example1.sh',
     dag=etl_dag)

task2 = BashOperator(
     task_id='second',
     bash_command='echo ok',
     dag=etl_dag)

#define the task order
task1 >> task2

#same would be task2 << task1
``` 
Now task1 will be execute firstly and, after its completion, task2.  
You can also mix upstream and downstream operators in the same workflow:

``` 
task1 >> task2 << task3

#is the same to this
task1 >> task2
task3 >> task2
``` 
Now task1 and task3 will be execute firstly and, after the completion of both, task2 will start.  

<br>


### Schedule
For scheduling we can use the Unix CRON syntax:
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1628087659991/VfQXKnfnc.png)
So:  
\* \* \* \* \* means run every seconds;  
0 12 \* \* \* means run daily at noon;  

```
from datetime import timedelta

default_args = {
  'owner': 'massy',
  'start_date': datetime(2021, 8, 4),
}

#run every friday at 12.30 pm
dag = DAG('update_dataflows', default_args=default_args, schedule_interval='30 12 * * 5')
``` 
<br>

### Run DAGs

To run a specific task in shell:  
airflow run [dag id] [task id] [start date] 
```
airflow run etl_dag move_file 2021-08-04
``` 

To run a full DAG:  
airflow trigger_dag -e [date] [dag_id]
```
airflow trigger_dag -e 2021-08-04 etl_dag
``` 

Some other shell command for airflow:
* airflow -h -> description
* airflow list_dags -> show all recognized DAGs, the executor used and if there are errors
* airflow scheduler -> run the scheduler


<br>

### Executors
Executors run tasks.  
Some executors:
* *SequentialExecutor* -> default, runs only a single task at the time
* *LocalExecutor* -> runs on a single system, can execute multiple tasks in parallel
* *CeleryExecutor* -> multiple Airflow systems can be configured as workers for a given set of workflows/tasks. Really powerful  
It's also possible to create executors.  

You can determine your executor by looking the airflow.cfg file and search for the *executor=* line, it will specify the executor in use
```
cat airflow/airflow.cfg | grep "executor = "

# executor = SequentialExecutor
``` 

<br>

### SLAs
SLA is the amount of time a task or a DAG should require to run.  
An SLA Miss is any time the task/DAG doesn't meet the expected timing.  
If a SLA is missed a log is stored.  
You can define SLAs using the *sla* argument on the task, or on the *default_args* dictionary, where you can also set to send emails after failure or success.  

```
from datetime import timedelta

#sla argument
task1 = BashOperator(task_id='sla_task',
     bash_command='example.sh',
     sla=timedelta(minutes=30),
     dag=etl_dag)

#default_args dictionary
default_args={
     'sla': timedelta(minutes=20),
     'start_date': datetime(2020,2,20),
     'email': 'massy@example.com',
     'email_on_failure': True,
     'email_on_success': True
}

dag = DAG('sla_dag', default_args=default_args)
``` 


<br>

### Variables
Airflow built-in runtime variables.
Some examples:
* {{ ds }} -> execution date YYYY-MM-DD
* {{ ds_nodash }} -> YYYYMMDD
* {{ dag }} -> DAG object

<br>

### Templates
Templates allow substitution of information during a DAG run, so you can add flexibility when defining task.  
Templates are created using the jinja templating language.  

```
#iterate the same operation for multiple file
templated_command = """
{% for filename in params.filenames %}
     echo "Processing {{ filename }}
{% endfor %}
"""

t1 = BashOperator(
     task_id='template_task',
     bash_command=templated_command,
     params={'filenames'= ['example1.txt', 'example2.txt', 'example3.txt']},
     dag=etl_dag)


#another example
filelist = [f'file{x}.txt' for x in range(30)]

templated_command = """
     <% for filename in params.filenames %>
     bash example.sh {{ ds_nodash }} {{ filename }};
     <% endfor %>
"""

clean_task = BashOperator(task_id='example_task',
                          bash_command=templated_command,
                          params={'filenames': filelist},
                          dag=example_dag)
``` 

<br>

### Branching
For conditional logic, takes a python_callable to return the next task id to follow.  

```
from airflow.operators.python_operator import BranchPythonOperator

#different executions in even or odd date
def branch_to_use(**kwargs):
     if int(kwargs['ds_nodash']) % 2 == 0:
          return 'even_task'
     else:
          return 'odd_task'

branch_task = BranchPythonOperator(
     task_id='branch_task',
     provide_context=True,
     python_callable=branch_to_use,
     dag=etl_dag)

#define the dependencies in both cases
start_task >> branch_task >> even_task >> even_day_task2
branch_task >> odd_task >> odd_day_task2
``` 
<br>

### A full example

```
from airflow.models import DAG
from airflow.contrib.sensors.file_sensor import FileSensor
from airflow.operators.bash_operator import BashOperator
from airflow.operators.python_operator import PythonOperator
from airflow.operators.python_operator import BranchPythonOperator
from airflow.operators.dummy_operator import DummyOperator
from airflow.operators.email_operator import EmailOperator
from dags.process import process_data
from datetime import datetime, timedelta

#Update the default arguments and apply them to the DAG.
default_args = {
  'start_date': datetime(2021,8,6),
  'sla': timedelta(minutes=90)
}
    
ex_dag = DAG(dag_id='etl_up', default_args=default_args)

#check for a file
sensor = FileSensor(task_id='sense_file', 
                    filepath='/home/repl/workspace/startprocess.txt',
                    poke_interval=45,
                    dag=ex_dag)

#exec a command
bash_task = BashOperator(task_id='cleanup_tempfiles', 
                         bash_command='rm -f /home/repl/*.tmp',
                         dag=ex_dag)

#exec a python script
python_task = PythonOperator(task_id='run_processing', 
                             python_callable=process_data,
                             provide_context=True,
                             dag=ex_dag)

#email
email_subject="""
  Email report for {{ params.dep }} on {{ ds_nodash }}
"""

email_task = EmailOperator(task_id='email_task',
                                  to='massy@example.com',
                                  subject=email_subject,
                                  html_content='',
                                  params={'dep': 'Data Mining department'},
                                  dag=ex_dag)

#no email
no_email_task = DummyOperator(task_id='no_email_task', dag=ex_dag)

#if weekend don't send email
def check_weekday(**kwargs):
    dt = datetime.strptime(kwargs['execution_date'],"%Y-%m-%d")
    if (dt.weekday() < 5):     #mon-fri
        return 'email_report_task'
    else:     #sat-sun
        return 'no_email_task'
    
branch_task = BranchPythonOperator(task_id='check_day',
                                   provide_context=True,
                                   python_callable=check_weekday,
                                   dag=ex_dag)


#define dependencies  
sensor >> bash_task >> python_task

python_task >> branch_task >> [email_task, no_email_task]
``` 




