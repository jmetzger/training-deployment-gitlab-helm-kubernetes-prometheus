# Defining and using (job) artifacts 

## What is it ? 

```
Jobs can output an archive of files and directories. This output is known as a job artifact.
You can download job artifacts by using the GitLab UI or the API.
```

## Example: Creating an artifact 

```
# .gitlab-ci.yml 

stages: 
  - build 

create_txt:
  stage: build 
  script:
    - echo "hello" > ergebnis.txt 
  artifacts:
    paths:
      - ergebnis.txt

```

## Example creating artifacts with wildcards and different name 

```

# .gitlab-ci.yml 
stages: 
  - build 
create_txt:
  stage: build 
  script:
    - mkdir -p path/my-xyz    
    - echo "hello" > path/my-xyz/ergebnis.txt
    - mkdir -p path/some-xyz
    - echo "some" > path/some-xyz/testtext.txt
  artifacts:
    name: meine-daten 
    paths:
      - path/*xyz/*

```

## Artifakten und Name aus Variable vergeben 

  * If your branch-name contains forward slashes
    * (for example feature/my-feature) 
    * it’s advised to use $CI_COMMIT_REF_SLUG instead of $CI_COMMIT_REF_NAME 
      * for proper naming of the artifact.

```
# .gitlab-ci.yml 
stages: 
  - build 
create_txt:
  stage: build 
  script:
    - mkdir -p path/my-xyz    
    - echo "hello" > path/my-xyz/ergebnis.txt
    - mkdir -p path/some-xyz
    - echo "some" > path/some-xyz/testtext.txt
  artifacts:
    name: "$CI_JOB_NAME-$CI_COMMIT_REF_NAME" 
    paths:
      - path/*xyz/*


```

## Alle files in einem Verzeichnis recursive 

```
# .gitlab-ci.yml 
stages: 
  - build 
create_txt:
  stage: build 
  script:
    - mkdir -p path/my-xyz    
    - echo "toplevel" > path/you-got-it.txt
    - echo "hello" > path/my-xyz/ergebnis.txt
    - mkdir -p path/some-xyz
    - echo "some" > path/some-xyz/testtext.txt 
  artifacts:
    paths:
      - path/


```

## Artifakte und Bedingungen 

```
# nur artifact erstellen, wenn ein commit-tag gesetzt ist. 
# Gibt es kein commit-tag ist diese Variable NICHT GESETZT. 


## .gitlab-ci.yml 
stages: 
  - build 

output_something:
  stage: build
  script:
    - echo "just writing something"
    - env
    - echo "CI_COMMIT_TAG:..$CI_COMMIT_TAG.."

create_txt:
  stage: build 
  script:
    - mkdir -p path/my-xyz    
    - echo "toplevel" > path/you-got-it.txt
    - echo "hello" > path/my-xyz/ergebnis.txt
    - mkdir -p path/some-xyz
    - echo "some" > path/some-xyz/testtext.txt 
    - env
  artifacts:
    paths:
      - path/

  rules:
    - if: $CI_COMMIT_TAG

```

  * Test 1: committen und Pipeline beobachten 
  * Test 2: Tag über repository > Tags erstellen und nochmal Pipeline beobachten  

## Using the gitlab - artifacts api 



### API - Reference:

  * https://docs.gitlab.com/ee/api/job_artifacts.html



## Reference:

  * https://docs.gitlab.com/ee/ci/pipelines/job_artifacts.html
