# Overview gitlab ci/cd 

## Pipelines 

  * The foundation of ci/cd are the pipelines 
  * You can either have preconfigured pipelines (using Auto DevOps) 
  * Or you can
    * Adjust them yourself (from Auto Devops, templates) 
    * Create one from scratch
  * Pipelines are either defined by Auto Devops or:
    * By .gitlab-ci.yml - file in the root-level - folder of your project   
  * There is also an editor under CI/CD -> Editor 

### Type of pipelines: Basic Pipeline 

  * Image: https://docs.gitlab.com/ee/ci/pipelines/pipeline_architectures.html#basic-pipelines
  * (each stage runs concurrently) 
  * Default behaviour 

```
# Example: 
stages:
  - build
  - test
  - deploy

image: alpine

build_a:
  stage: build
  script:
    - echo "This job builds something."

build_b:
  stage: build
  script:
    - echo "This job builds something else."

test_a:
  stage: test
  script:
    - echo "This job tests something. It will only run when all jobs in the"
    - echo "build stage are complete."

test_b:
  stage: test
  script:
    - echo "This job tests something else. It will only run when all jobs in the"
    - echo "build stage are complete too. It will start at about the same time as test_a."

deploy_a:
  stage: deploy
  script:
    - echo "This job deploys something. It will only run when all jobs in the"
    - echo "test stage complete."

deploy_b:
  stage: deploy
  script:
    - echo "This job deploys something else. It will only run when all jobs in the"
    - echo "test stage complete. It will start at about the same time as deploy_a."

```

### Type of pipelines: DAG (Directed Acyclic Graph) Pipelines

  * Image: 
  * Deploy_a can run, although build_b->test_b is not evening ready
  * Because gitlab knows the dependencies by keyword: needs: 

```
# Example:
stages:
  - build
  - test
  - deploy

image: alpine

build_a:
  stage: build
  script:
    - echo "This job builds something quickly."

build_b:
  stage: build
  script:
    - echo "This job builds something else slowly."

test_a:
  stage: test
  needs: [build_a]
  script:
    - echo "This test job will start as soon as build_a finishes."
    - echo "It will not wait for build_b, or other jobs in the build stage, to finish."

test_b:
  stage: test
  needs: [build_b]
  script:
    - echo "This test job will start as soon as build_b finishes."
    - echo "It will not wait for other jobs in the build stage to finish."

deploy_a:
  stage: deploy
  needs: [test_a]
  script:
    - echo "Since build_a and test_a run quickly, this deploy job can run much earlier."
    - echo "It does not need to wait for build_b or test_b."

deploy_b:
  stage: deploy
  needs: [test_b]
  script:
    - echo "Since build_b and test_b run slowly, this deploy job will run much later."


```

### Type of pipelines: Child- / Parent - Pipelines 

  * https://docs.gitlab.com/ee/ci/pipelines/pipeline_architectures.html#child--parent-pipelines
  * in Example: two types of things that could be built independently.
  * Combines child and DAG in this case 
  * Trigger is used to start the child - pipeline 
  * Include not to repeat yourself 
  * Rules: are like conditions 

```
# Example 
# File 1: .gitlab-ci.yml 
stages:
  - triggers

trigger_a:
  stage: triggers
  trigger:
    include: a/.gitlab-ci.yml
  rules:
    - changes:
        - a/*

trigger_b:
  stage: triggers
  trigger:
    include: b/.gitlab-ci.yml
  rules:
    - changes:
        - b/*
```

```
# File 2: a/.gitlab-ci.yml 
stages:
  - build
  - test
  - deploy

image: alpine

build_a:
  stage: build
  script:
    - echo "This job builds something."

test_a:
  stage: test
  needs: [build_a]
  script:
    - echo "This job tests something."

deploy_a:
  stage: deploy
  needs: [test_a]
  script:
    - echo "This job deploys something."
```

```
# File 3: a/.gitlab-ci.yml 
stages:
  - build
  - test
  - deploy

image: alpine

build_b:
  stage: build
  script:
    - echo "This job builds something else."

test_b:
  stage: test
  needs: [build_b]
  script:
    - echo "This job tests something else."

deploy_b:
  stage: deploy
  needs: [test_b]
  script:
    - echo "This job deploys something else."
```

## Type of pipelines: Ref:

  * https://docs.gitlab.com/ee/ci/pipelines/pipeline_architectures.html


## Stages 

  * Stages run one after each other 
  * They default to: build, test, deploy (if you do not define any) 
  * If you want to have less, you have to define which 
  * Reference: 

## Jobs 

  * Jobs define what to do within the stages 
  * Normally jobs are run concurrently in each stage 
  * Reference:


