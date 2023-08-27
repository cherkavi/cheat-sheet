# [CircleCI](https://circleci.com/docs/getting-started/)
![circle-ci](https://i.ibb.co/3NJ5wqL/2023-08-27-2git-circle-ci.jpg)  

## setup
* [token](https://app.circleci.com/settings/user/tokens)
* visual code extension: circleci.circleci
* [install cli](https://circleci.com/docs/local-cli/)
    ```sh
    sudo snap install circleci
    circleci setup
    # update your ~/.bashrc
    eval "$(circleci completion bash)"
    export CIRCLECI_CLI_HOST=https://circleci.com
    
    circleci setup
    ```

## working loop
* [docker images for job run](https://hub.docker.com/u/cimg)
* [docker images for job run](https://hub.docker.com/u/circleci/)
* [orbs - shared configuration ](https://circleci.com/developer/orbs)
  - [orbs intro](https://circleci.com/docs/orb-intro/)
* [minimal example](https://circleci.com/docs/sample-config/)
* [list of possible step-commands like: run,checkout... ](https://circleci.com/docs/configuration-reference/#steps)
  - [run](https://circleci.com/docs/configuration-reference/#run)
  - [checkout](https://circleci.com/docs/configuration-reference/#checkout)
  - [save_cache](https://circleci.com/docs/configuration-reference/#savecache)
  - [persist_to_workspace](https://circleci.com/docs/configuration-reference/#persisttoworkspace)
  - [attach_workspace](https://circleci.com/docs/configuration-reference/#attachworkspace)
  - [deploy](https://circleci.com/docs/configuration-reference/#deploy)
  - [docker](https://circleci.com/docs/configuration-reference/#docker)
  - [add_ssh_keys](https://circleci.com/docs/configuration-reference/#addsshkeys)
  - [add_ssh_known_hosts](https://circleci.com/docs/configuration-reference/#addsshknownhosts)
  - [machine](https://circleci.com/docs/configuration-reference/#machine)
  - [store_artifacts](https://circleci.com/docs/configuration-reference/#storeartifacts)
  - [store_test_results](https://circleci.com/docs/configuration-reference/#storetestresults)
  - ...
  
## setup new project
1. create repository in github/gitlab
2. create file `.circleci/config.yml` in repository
```yaml
# CircleCI configuration file
version: 2.1

jobs:
  # first job
  print_hello:
    docker:
      - image: cimg/base:2022.05
    steps:
      - run: echo "--- 1.step"
      - run: echo "hello"
      - run: echo "----------"

  # second job
  print_world:
    docker:
     - image: cimg/base:2022.05
    steps:
      - run: 
          name: print world
          command: | 
            echo "--- 2.step"
            echo "world"
            echo "----------"
 
workflows:
  # Name of workflow
  my_workflow_1:
    jobs:
      - print_hello
      - print_world:
          requires: 
            - print_hello
```
```bash
circleci validate
```
3. [connect circleci to project](https://app.circleci.com/projects/connect-vcs/)
> webhook will be created in the project

## try catch job
### try catch on job level
```yaml
jobs:
    job_name:
        docker:
            - image: cimg/base:2022.05
        steps:
            - run: echo "hello"
            - run: 
                name: error simulation
                command: |  
                    return 1
            - run:
                name: catch possible previous errors
                command: |
                    echo "time to execute rollback operation"
                when: on_fail
```
other possible conditions:
```
when:
    environment:
        MY_VAR: "true"
```
```
when:
    # of previous step
    status: success
```
```
when:
    branch:
        only:
            - master
            - develop
```
```
when: |
    steps.branch.matches('feature/*') || steps.branch.matches('bugfix/*')
```

### try catch on command level
```yaml
jobs:
    job_name:
        docker:
            - image: cimg/base:2022.05
        steps:
            - run: 
                name: error simulation
                command: |  
                    return 1
                on_fail:
                    - run:
                        name: catch exception
                        command: |
                            echo " time to execute rollback operation"
```

### job examples
#### job cloudformation
```
  run_cloudformation: 
    docker:
      - image: amazon/aws-cli
    steps:
      - checkout
      - run:
          name: run cloudformation
          command: |
            aws cloudformation deploy \
              --template-file my-template-file.yml \
              --stack-name my-template-file-${CIRCLE_WORKFLOW_ID:0:5} \
              --region us-east-1
```

## [pipeline variables](https://circleci.com/docs/pipeline-variables/#pipeline-values)
```yaml
jobs:
    job_name:
        docker: 
        - image: cimg/base:2022.05
        environment:
            CUSTOM_MESSAGE: "<< pipeline.project.git_url >> :: << pipeline.trigger_parameters.gitlab.commit_message>>"
        steps:
        - run: echo "$CUSTOM_MESSAGE"
```
### [pipeline parameters](https://circleci.com/docs/pipeline-variables/#pipeline-parameters-in-configuration)
```yaml
version: 2.1

parameters:
  my_custom_parameter_1:
    type: string
    default: "~/"

jobs:
    job_name:
        docker: 
        - image: cimg/base:2022.05
        environment:
            CUSTOM_MESSAGE: "<< pipeline.parameters.my_custom_parameter_1 >>"
```

### job with parameters
```yaml
jobs:
  my_job_1:
    parameters:
      my_custom_parameter_1:
        type: string
        default: my_org
    docker:
      - image: cimg/base:2020.01
    steps:
      - run: echo "  << parameters.my_custom_parameter_1 >>  "

workflows:
  my_workflow_1:
    jobs:
      - my_job_1:
          my_custom_parameter_1: my_organization          
      - my_job_1:
          my_custom_parameter_1: another organization
```

## [environment variables](https://circleci.com/docs/env-vars/)
> can be installed from: Project Settings -> Environment Variables
> all env variables will be hidden as secrets ( no output in console will be visible)

## triggering pipeline
* webhook
  * new commit in branch
  * pull/merge request
* api
  * command-line tool
  * chat message
* schedule
* other pipeline

## jobs communication
### cache ( folders & files )
> ttl - only during run of pipeline 
> immutable after creation
```yaml
# https://circleci.com/docs/configuration-reference/#savecache
- save_cache:
    key: |
      maven_repo-{{ .Branch }}-{{ .Revision }} 
      result_of_build-{{ .Branch }}-{{ .Revision }}
    paths:
      - ~/.m2
      - target/application-with-dependencies.jar
```
```yaml
# https://circleci.com/docs/configuration-reference/#restorecache
- restore_cache:
    keys:
      - maven_repo-{{ .Branch }}-{{ .Revision }} 
      - result_of_build-{{ .Branch }}-{{ .Revision }}
```

### workspace
> mutable
```yaml
# https://circleci.com/docs/configuration-reference/#persisttoworkspace
- persist_to_workspace:
    root: /tmp/my_folder_1
    path:
      - target/appliaction.jar
      - src/config/settings.properties
```
```yaml
# https://circleci.com/docs/configuration-reference/#attachworkspace
- attach_workspace:
    at: /tmp/my_folder_1
```
### secret keeper
* Vault (HashiCorp's)
* https://kvdb.io
* deprecated: metstash.io

## job manual approval
```yaml
      - hold: 
          type: approval # "On Hold"
```

## Local usage
### how to execute job
```sh
JOB_NAME=job_echo
circleci local execute --job $JOB_NAME
```
