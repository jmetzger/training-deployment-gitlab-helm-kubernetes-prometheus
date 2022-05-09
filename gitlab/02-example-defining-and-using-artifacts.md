# Defining and using (job) artifacts 

## What is it ? 

```
Jobs can output an archive of files and directories. This output is known as a job artifact.
You can download job artifacts by using the GitLab UI or the API.
```

## Example: Creating an artifact 

```
# .gitlab-ci.yml 

# only one stage - keine Liste 
stages: 
  - build 

create_txt:
  stage: build 
  script:
    - echo "hello" > ergebnis.txt 
  artifacts:
    - ergebnis.txt

```

## Example creating artifacts with wildcards 

```

job:
  artifacts:
    paths:
      - path/*xyz/*





```

## Artifakte und Bedingungen 

```



```



## Using the gitlab - artifacts api 



### API - Reference:

  * https://docs.gitlab.com/ee/api/job_artifacts.html



## Reference:

  * https://docs.gitlab.com/ee/ci/pipelines/job_artifacts.html
