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
