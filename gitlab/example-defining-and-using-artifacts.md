# Defining and using (job) artifacts 

## What is it ? 

```
Jobs can output an archive of files and directories. This output is known as a job artifact.
You can download job artifacts by using the GitLab UI or the API.
```

## Example Walkthrough 

```
# Schritt 1: Neues Repo aufsetzen 

# Setup a new repo 
# Setting:

# o Public, dann bekommen wir mehr Rechenzeit 
# o  No deployment planned 
# o No SAST 
# o Add README.md 

# Using naming convention 
# Name it however you want, but have you tln - nr inside 
# e.g.
# test-artifacts-tln1


# Schritt 2: Ein Standard-Template als Ausgangsbasis holen 
# Get default ci-Template 
CI-CD -> Pipelines -> Try Test-Template 

# Testtemplate wird in file gitlab-ci.yaml angelegt. 
# Es erscheint unter: CI-CD -> Editor 

1x speichern und committen.

# Jetzt wird es in der Pipeline ausgeführt. 

```



## Using the gitlab - artifacts api 



### Reference:

  * https://docs.gitlab.com/ee/api/job_artifacts.html



## Reference:

  * https://docs.gitlab.com/ee/ci/pipelines/job_artifacts.html
