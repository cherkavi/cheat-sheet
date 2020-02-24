# ![Airflow](https://airbnb.io/img/projects/airflow3.png)
* [Airflow apache](https://airflow.apache.org/)
* [how to](https://airflow.apache.org/howto/index.html)
* [podcast](https://soundcloud.com/the-airflow-podcast)
* [tutorial](https://github.com/hgrif/airflow-tutorial)

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
    * catchup ( config:catchup_by_default ) or "BackFill" ( fill previous executions from start_date )
* Executor
  * type: LocalExecutor, SequentialExecutor, CeleryExecutor, DaskExecutor
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
### [multi-tasks](https://github.com/cherkavi/cheat-sheet/blob/master/development-process.md#concurrency-vs-parallelism)
```
# number of physical python processes the scheduler can run
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
```properties
executor = CeleryExecutor
sql_alchemy_conn = postgresql+psycopg2://airflow@localhost:5432/airflow_metadata
broker_url = pyamqp://admin:rabbitmq@localhost/
result_backend = db+postgresql://airflow@localhost:5432/airflow_metadata
worker_log_server_port = 8899
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
