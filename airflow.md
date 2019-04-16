# ![Airflow](https://airbnb.io/img/projects/airflow3.png)
* [Airflow apache](https://airflow.apache.org/)
* [how to](https://airflow.apache.org/howto/index.html)
* [podcast](https://soundcloud.com/the-airflow-podcast)

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
