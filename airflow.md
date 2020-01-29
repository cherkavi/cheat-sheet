# ![Airflow](https://airbnb.io/img/projects/airflow3.png)
* [Airflow apache](https://airflow.apache.org/)
* [how to](https://airflow.apache.org/howto/index.html)
* [podcast](https://soundcloud.com/the-airflow-podcast)
* [tutorial](https://github.com/hgrif/airflow-tutorial)

* [components](https://github.com/astronomer/airflow-guides/blob/master/guides/airflow-components.md)
## Key concepts
* DAG
a graph object representing your data pipeline.  
Should be:
  * idempotent ( execution of many times without side effect )
  * can be retried automatically
* Operator
Describe a single task in your data pipeline
* Task
An instance of an operator
* Task Instance
Represents a specific run of a task = DAG + Task + Point of time
* Workflow
Combination of all above

## Architecture overview
![single node](https://i.postimg.cc/3xzBzNCm/airflow-architecture-singlenode.png)
![multi node](https://i.postimg.cc/MGyy4DGJ/airflow-architecture-multinode.png)
![statuses](https://i.postimg.cc/g2kd76Z5/airflow-statuses.png)

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
airflow scheduler &
airflow webserver -p 8080 &
echo "localhost:8080"

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

## trigger DAG - bash
```sh
curl -X POST --user tech-user \     
    --data '{"conf":{"session_id": "bff2-08275862a9b0"}}' \
    https://airflow.local/api/experimental/dags/ibeo_gt/dag_runs

# check running
curl -X GET --user ibeo_gt-s https://airflow.local/api/experimental/dags/ibeo_gt/dag_runs | jq '.[] | if .state=="running" then . else empty end'
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

default_arguments = {
    'owner': 'test-datetime-now',
    'start_date': datetime.now(),
    'retries': 1,
    'retry_delay': timedelta(minutes=5),
    'catchup': False
}

with DAG('dummy_echo_dag',
         default_args=default_arguments,
         schedule_interval='*/15 * * * *'
         ) as dag:
    print(dag)
```

* reading settings files ( dirty way )
```python
# settings.json should be placed in the same folder as dag description
# configuration should contains: dags_folder = /usr/local/airflow/dags
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
