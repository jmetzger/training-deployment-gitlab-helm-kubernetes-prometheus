# 08 gitlab ci examples 

## Example 1: For debugging purposes 

```
# .gitlab-ci.yml 
stages:          # List of stages for jobs, and their order of execution
  - deploy

deploy-job:      # This job runs in the deploy stage.
  stage: deploy  # It only runs when *both* jobs in the test stage complete successfully.
  script:
    - echo "Deploying application..."
    - pwd
    - ls -la
    - env
    - id

```

## Example 2: Using different image 

```

# .gitlab-ci.yml 
stages:          # List of stages for jobs, and their order of execution
  - deploy

deploy-job:      # This job runs in the deploy stage.
  stage: deploy  # It only runs when *both* jobs in the test stage complete successfully.
  image: 
    name: bitnami/kubectl:latest
    entrypoint: [""]
    
  script:
    - echo "Deploying application..."
    - pwd
    - ls -la
    - env
    - id
    - apt get update

```

## Example 3: Manually start stage 

```

stages:          # List of stages for jobs, and their order of execution
  - deploy

deploy-job:      # This job runs in the deploy stage.
  stage: deploy  # It only runs when *both* jobs in the test stage complete successfully.
#  image: 
#    name: bitnami/kubectl:latest
#    entrypoint: [""]
    
  script:
    - echo "Deploying application..."
    - pwd
    - ls -la
    - env
    - id
    - chmod u+x run.sh
    - ./run.sh
  when: manual
  
```
