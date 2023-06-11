# [dbt - data build tool](https://docs.getdbt.com/docs/introduction)
> create, transform and validate data within Warehouse  
![dbt simple architecture](https://github.com/dbt-labs/dbt-core/raw/202cb7e51e218c7b29eb3b11ad058bd56b7739de/etc/dbt-transform.png)

## dbt tutorials links:
* [what is dbt, dbt tutorial](https://www.startdataengineering.com/post/dbt-data-build-tool-tutorial/)
* [dbt core quick start](https://docs.getdbt.com/quickstarts/codespace?step=1)
* [dbt trainings](https://www.getdbt.com/dbt-learn/)
  
## [dbt local installation main commands](https://github.com/HarunMbaabu/Data-Build-Tool-Ultimate-Guide)
```sh
pip3 install dbt-core
dbt --help

# init new project in current folder
dbt init 

# run dbt models in the project
dbt run

# run tests
dbt tests

## target database manipulations
# clean target db
dbt clean
# create snapshot of the data
dbt snapshot
# load seeds data
dbt seed
```