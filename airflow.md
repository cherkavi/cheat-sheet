# ![Airflow](https://airbnb.io/img/projects/airflow3.png)
* [Airflow apache](https://airflow.apache.org/)
* [how to](https://airflow.apache.org/howto/index.html)
* [podcast](https://soundcloud.com/the-airflow-podcast)
* [tutorial](https://github.com/hgrif/airflow-tutorial)
* [blog](https://marclamberti.com/blog/)
* [examples](https://github.com/cherkavi/docker-images/blob/master/airflow/airflow-dag-examples.zip)

* [components](https://github.com/astronomer/airflow-guides/blob/master/guides/airflow-components.md)
## Key concepts
* DAG
a graph object representing your data pipeline ( collection of tasks ).  
Should be:
  * idempotent ( execution of many times without side effect )
  * can be retried automatically
* [Operator](https://airflow.apache.org/docs/1.10.1/howto/operator.html) describe a single task in your data pipeline
 * **action** - perform actions ( airflow.operators.BashOperator, airflow.operators.PythonOperator, airflow.operators.EmailOperator... )
 * **transfer** - move data from one system to another ( SftpOperator, S3FileTransformOperator, MySqlOperator, SqliteOperator, PostgresOperator, MsSqlOperator, OracleOperator, JdbcOperator, airflow.operators.HiveOperator.... )
 ( don't use it for BigData - source->executor machine->destination )
 * **sensor** - waiting for arriving data to predefined location ( airflow.contrib.sensors.file_sensor.FileSensor )
 has a method #poke that is calling repeatedly until it returns True
* Task
An instance of an operator
* Task Instance
Represents a specific run of a task = DAG + Task + Point of time
* Workflow
Combination of Dags, Operators, Tasks, TaskInstances

## Architecture overview
![single node](https://i.postimg.cc/3xzBzNCm/airflow-architecture-singlenode.png)
![multi node](https://i.postimg.cc/MGyy4DGJ/airflow-architecture-multinode.png)
![statuses](https://i.postimg.cc/g2kd76Z5/airflow-statuses.png)

### components
* WebServer  
  * read user request  
  * UI  
* Scheduler 
  * scan folder "%AIRFLOW%/dags" ( config:dag_folder ) and with timeout ( config:dag_dir_list_interval )
  * monitor execution "start_date" + "schedule_interval", write "execution_date" ( last time executed )
  * create DagRun ( instance of DAG ) and fill DagBag ( with interval config:worker_refresh_interval )
    * start_date ( start_date must be in past, start_date+schedule_interval must be in future )
    * end_date
    * retries
    * retry_delay
    * schedule_interval (cron:str / datetime.timedelta) ( cron presets: @once, @hourly, @daily, @weekly, @monthly, @yearly )
    * catchup ( config:catchup_by_default ) or "BackFill" ( fill previous executions from start_date ) actual for scheduler only
* Executor
  * type: LocalExecutor(multiply task in parallel), SequentialExecutor, CeleryExecutor, DaskExecutor
* Metadatabase
  * [types](https://docs.sqlalchemy.org/en/13/dialects/index.html)
  * configuration:
    * sql_alchemy_conn
    * sql_alchemy_pool_enabled
   

## [Airflow install on python virtualenv]
```sh
# create python virtual env
python3 -m venv airflow-env
source airflow-env/bin/activate

# create folder 
mkdir airflow
export AIRFLOW_HOME=`pwd`/airflow

# install workflow
pip install apache-airflow
```

## [Airflow start on python, nacked start, start components, start separate components, start locally]
```sh
# init workflow
airflow initdb 
# airflow resetdb - for reseting all data 
airflow scheduler &
airflow webserver -p 8080 &
echo "localhost:8080"

# check logs
airflow serve_logs

# sudo apt install sqllite3
# sqllite3 $AIRFLOW_HOME/airflow.db
```

## [Airflow docker](https://github.com/cherkavi/docker-images/tree/master/airflow)
```sh
# * copy your dags to ``` .dags```
docker-compose -f docker-compose-LocalExecutor.yml up -d
```

## [Airflow virtual environment](https://github.com/hgrif/airflow-tutorial)
```sh
python env create -f environment.yml
source activate airflow-tutorial
```
  
## [Airflow Virtual machine](https://marclamberti.com/form-course-material-100/)
credentials
```sh
ssh -p 2200 airflow@localhost
# passw: airflow
```
activate workspace
```sh
source .sandbox/bin/activate
```
## commands
check workspace
```sh
airflow --help
```

## operator types ( BaseOperator )
* action
* transfer ( data )
* sensor ( waiting for some event )
   * long running task
   * BaseSensorOperator
   * poke method is responsible for waiting

# Access to DB
```gui
Admin -> Connections -> postgres_default  
# adjust login, password
Data Profiling->Ad Hoc Query-> postgres_default  
```
```sql
select * from dag_run;
```

# REST API
## trigger DAG - python
```python
import urllib2
import json

AIRFLOW_URL="https://airflow.local/api/experimental/dags/name_of_my_dag/dag_runs"
payload_dict = {"conf": {"dag_param_1": "test value"}}

req = urllib2.Request(AIRFLOW_URL, data=json.dumps(payload_dict))
req.add_header('Content-Type', 'application/json')
req.add_header('Cache-Control', 'no-cache')
req.get_method = lambda: "POST"
f = urllib2.urlopen(req)
print(f.read())
```

## trig DAG
```sh
curl -X POST --user tech-user \     
    --data '{"conf":{"session_id": "bff2-08275862a9b0"}}' \
    https://airflow.local/api/experimental/dags/ibeo_gt/dag_runs

# check running
curl -X GET --user ibeo_gt-s https://airflow.local/api/experimental/dags/ibeo_gt/dag_runs | jq '.[] | if .state=="running" then . else empty end'
```

## configuration
### rewrite configuration with environment variables
example of overwriting configuration from config file by env-variables
```properties
[core]
airflow_home='/path/to/airflow'
dags_folder='/path/to/dags'
```
```bash
AIRFLOW__CORE__DAGS_FOLDER='/path/to/new-dags-folder'
AIRFLOW__CORE__AIRFLOW_HOME='/path/to/new-version-of-airflow'
```

### [multi-tasks](https://github.com/cherkavi/cheat-sheet/blob/master/development-process.md#concurrency-vs-parallelism)
```
# number of physical python processes the scheduler can run, task (processes) that running in parallel 
parallelism

# number of DagRuns - will be concurrency in dag execution, don't use in case of dependencies of dag-runs
max_active_runs_per_dag

# number of tast instances that are running simultaneously per DagRun ( amount of TaskInstances inside one DagRun )
dag_concurrency
```

### different configuration of executor
### LocalExecutor with PostgreSQL
```properties
executor = LocalExecutor
sql_alchemy_conn = postgresql+psycopg2://airflow@localhost:5432/airflow_metadata
```
### CeleryExecutor with PostgreSQL and RabbitMQ ( recommended for prod )
settings
```properties
executor = CeleryExecutor
sql_alchemy_conn = postgresql+psycopg2://airflow@localhost:5432/airflow_metadata
# RabbitMQ UI: localhost:15672
broker_url = pyamqp://admin:rabbitmq@localhost/
result_backend = db+postgresql://airflow@localhost:5432/airflow_metadata
worker_log_server_port = 8899
```
start Celery worker node
```sh
# just a start worker process
airflow worker
# start with two child worker process - the same as 'worker_concurrency" in airflow.cfg
airflow worker -c 2 
```

## DAG
### task dependencies in DAG
```python
# Task1 -> Task2 -> Task3
t1.set_downstream(t2);t2.set_downstream(t3)
t1 >> t2 >> t3

t3.set_upstream(t2);t2.set_upstream(t1)
t3 << t2 << t1
```

### task information, task metainformation, task context
```python
def python_operator_core_func(**context):
   print(context['task_instance'])
   context["dag_run"].conf['dag_run_argument']
   # the same as previous
   # manipulate with task-instance inside custom function, context inside custom function
   context.get('ti').xcom_push(key="k1", value="v1")
...   
PythonOperator(task_id="python_example", python_callable=python_operator_core_func, provide_context=True )
```

### sub-dags
```python
from airflow.operators.subdag_operator import SubDagOperator
...
subdag_task = SubDagOperator(subdag=DAG(SUBDAG_PARENT_NAME+"."+SUBDAG_NAME,schedule_interval=parent_dag.schedule_interval, start_date=parent_dag.start_date,catchup=False))
...
```

### hooks
collaboration with external sources via "connections"  
* [official airflow hooks](https://airflow.apache.org/docs/stable/_api/airflow/hooks/index.html)
* [SparkSubmitHook, FtpHook, JenkinsHook....](https://airflow.apache.org/docs/stable/_modules/airflow/contrib/hooks.html)

### XCOM, Cross-communication
GUI: Admin -> Xcoms  
Should be manually cleaned up  
Exchange information between multiply tasks - "cross communication".  
<Key> <Value> <Timestamp>  
Object must be serializable  
Some operators ( BashOperator, SimpleHttpOperator, ... ) have parameter xcom_push=True - last std.output/http.response will be pushed  
Some operators (PythonOperator) has ability to "return" value from function ( defined in operator ) - will be automatically pushed to XCOM  
Saved in Metadabase, also additional data: "execution_date", "task_id", "dag_id"  
"execution_date" means hide(skip) everything( same task_id, dag_id... ) before this date  
```python
xcom_push(key="name_of_value", value="some value")
xcom_pull(task_ids="name_of_task_with_push")
```

### branching, select next step, evaluate next task
![branching](https://i.postimg.cc/T3pRNS4F/airflow-branching.png)
!!! don't use "depends_on_past"
```python
def check_for_activated_source():
  # return name ( str ) of the task
  return "mysql_task"

branch_task = BranchPythonOperator(task_id='branch_task', python_callable=check_for_activated_source)
mysql_task 	= BashOperator(task_id='mysql_task', bash_command='echo "MYSQL is activated"')
postgresql_task = BashOperator(task_id='postgresql_task', bash_command='echo "PostgreSQL is activated"')
mongo_task 	= BashOperator(task_id='mongo_task', bash_command='echo "Mongo is activated"')

branch_task >> mysql_task
branch_task >> postgresql_task
branch_task >> mongo_task
```

### Service Level Agreement, SLA
GUI: Browse->SLA Misses  
```python
def log_sla_miss(dag, task_list, blocking_task_list, slas, blocking_tis):
    print("SLA was missed on DAG {0}s by task id {1}s with task list: {2} which are " \
	"blocking task id {3}s with task list: {4}".format(dag.dag_id, slas, task_list, blocking_tis, blocking_task_list))

...

# call back function for missed SLA
with DAG('sla_dag', default_args=default_args, sla_miss_callback=log_sla_miss, schedule_interval="*/1 * * * *", catchup=False) as dag:
  t0 = DummyOperator(task_id='t0')
	 t1 = BashOperator(task_id='t1', bash_command='sleep 15', sla=timedelta(seconds=5), retries=0)
 	t0 >> t1
```

## [DAG examples](https://github.com/apache/airflow/tree/master/airflow/example_dags)
should be placed into "dag" folder ( default: %AIRFLOW%/dag )
* minimal
```python
from airflow import DAG

with DAG('airflow_tutorial_v01',
         default_args=default_args, 
         schedule_interval='0 * * * *',
         ) as dag:
    print(dag)
```

* simple
```python
from airflow import DAG
from datetime import date, timedelta, datetime
from airflow.models import BaseOperator
from airflow.operators.bash_operator import BashOperator

default_arguments = {
    'owner': 'airflow'
    ,'start_date': datetime(2016,1,1) # do not do that: datetime.now()
    ,'retries': 1
    #,'retry_delay': timedelta(minutes=5)
    #,'catchup': False - will be re-writed from ConfigFile !!!
    ,'depends_on_past': False
}

with DAG(dag_id='dummy_echo_dag_10'
         ,default_args=default_arguments
         ,schedule_interval="*/5 * * * *"
         ,catchup=False
         ) as dag:
    BashOperator(task_id='bash_example', bash_command="date", dag=dag)    
```

* reading settings files ( dirty way )
```python
# settings.json should be placed in the same folder as dag description
# configuration shoulhttps://github.com/cherkavi/cheat-sheet/blob/master/development-process.md#concurrency-vs-parallelismd contains: dags_folder = /usr/local/airflow/dags
def get_request_body():
    with open(f"{str(Path(__file__).parent.parent)}/dags/settings.json", "r") as f:
        request_body = json.load(f)
        return json.dumps(request_body)
```

* collaboration between tasks, custom functions
```python
COLLABORATION_TASK_ID="mydag_first_call"

def status_checker(resp):
    job_status = resp.json()["status"]
    return job_status in ["SUCCESS", "FAILURE"]

def cleanup_response(response):
    return response.strip()

def create_http_operator(connection_id=MYDAG_CONNECTION_ID):
    return SimpleHttpOperator(
        task_id=COLLABORATION_TASK_ID,
        http_conn_id=connection_id,
        method="POST",
        endpoint="v2/endpoint",
        data="{\"id\":111333222}",
        headers={"Content-Type": "application/json"},
        # response will be pushed to xcom with COLLABORATION_TASK_ID
        xcom_push=True,
        log_response=True,
    )


def second_http_call(connection_id=MYDAG_CONNECTION_ID):
    return HttpSensor(
        task_id="mydag_second_task",
        http_conn_id=connection_id,
        method="GET",
        endpoint="v2/jobs/{{ parse_response(ti.xcom_pull(task_ids='" + COLLABORATION_TASK_ID + "' )) }}",
        response_check=status_checker,
        poke_interval=15,
        depends_on_past=True,
        wait_for_downstream=True,
    )


with DAG(
    default_args=default_args,
    dag_id="dag_name",
    max_active_runs=1,
    default_view="graph",
    concurrency=1,
    schedule_interval=None,
    catchup=False,
    # custom function definition
    user_defined_macros={"parse_response": cleanup_response},
) as dag:
    first_operator = first_http_call()
    second_operator = second_http_call()
    first_operator >> second_operator
```

## Plugins
[official documentation](https://airflow.apache.org/docs/stable/plugins.html)  
[examples of airflow plugins](https://github.com/airflow-plugins)  
* Operators: They describe a single task in a workflow. Derived from BaseOperator.
* Sensors: They are a particular subtype of Operators used to wait for an event to happen. Derived from BaseSensorOperator
* Hooks: They are used as interfaces between Apache Airflow and external systems.  Derived from BaseHook
* Executors: They are used to actually execute the tasks. Derived from BaseExecutor
* Admin Views: Represent base administrative view from Flask-Admin allowing to create web
interfaces. Derived from flask_admin.BaseView (new page = Admin Views + Blueprint )
* Blueprints: Represent a way to organize flask application into smaller and re-usable application. A blueprint defines a collection of views, static assets and templates. Derived from flask.Blueprint (new page = Admin Views + Blueprint )
* Menu Link: Allow to add custom links to the navigation menu in Apache Airflow. Derived from flask_admin.base.MenuLink
* Macros: way to pass dynamic information into task instances at runtime. They are tightly coupled with Jinja Template.

plugin template
```python
# init.py

from airflow.plugins_manager import AirflowPlugin
from elasticsearch_plugin.hooks.elasticsearch_hook import ElasticsearchHook

# Views / Blueprints / MenuLinks are instantied objects
class MyPlugin(AirflowPlugin):
	name 			= "my_plugin"
	operators 		= [MyOperator]
	sensors			= []
	hooks			= [MyHook]
	executors		= []
	admin_views		= []
	flask_blueprints	= []
	menu_links		= []
```

```sh
my_plugin/
├── __init__.py
├── hooks
│   ├── my_hook.py
│   └── __init__.py
├── menu_links
│   ├── my_link.py
│   └── __init__.py
├── operators
    ├── my_operator.py
    └── __init__.py
```
