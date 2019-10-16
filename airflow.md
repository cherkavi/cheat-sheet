# ![Airflow](https://airbnb.io/img/projects/airflow3.png)
* [Airflow apache](https://airflow.apache.org/)
* [how to](https://airflow.apache.org/howto/index.html)
* [podcast](https://soundcloud.com/the-airflow-podcast)
* [tutorial](https://github.com/hgrif/airflow-tutorial)

* [components](https://github.com/astronomer/airflow-guides/blob/master/guides/airflow-components.md)
## Key concepts
* DAG
a graph object representing your data pipeline
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

## [Airflow install on python virtualenv]
```
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
```
python env create -f environment.yml
source activate airflow-tutorial
```

## [Airflow Virtual machine](https://marclamberti.com/form-course-material-100/)
credentials
```
ssh -p 2200 airflow@localhost
# passw: airflow
```
activate workspace
```
source .sandbox/bin/activate
```
## commands
check workspace
```
airflow --help
```

## DAG example
should be placed into "dag" folder ( default: %AIRFLOW%/dag )
* minimal
```
from airflow import DAG

with DAG('airflow_tutorial_v01',
         default_args=default_args, 
         schedule_interval='0 * * * *',
         ) as dag:
    print(dag)
```

* simple
```
from airflow import DAG
from datetime import date, timedelta, datetime

arguments = {
    'owner': 'me',
    'start_date': dt.datetime(2019, 4, 13),
    'retries': 1,
    'retry_delay': dt.timedelta(minutes=5),
}

with DAG('airflow_tutorial_v01',
         default_args=default_args, 
         schedule_interval='0 * * * *',
         default_args=arguments
         ) as dag:
    print(dag)
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
```
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
```
curl -X POST --user tech-user \     
    --data '{"conf":{"session_id": "bff2-08275862a9b0"}}' \
    https://airflow.local/api/experimental/dags/ibeo_gt/dag_runs

# check running
curl -X GET --user ibeo_gt-s https://airflow.local/api/experimental/dags/ibeo_gt/dag_runs | jq '.[] | if .state=="running" then . else empty end'
```
