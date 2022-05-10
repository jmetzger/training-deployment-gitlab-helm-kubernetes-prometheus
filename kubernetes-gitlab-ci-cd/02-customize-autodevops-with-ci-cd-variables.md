# Customize Auto DevOps with CI/CD - Variables 

## Disabling some features 

```
# You just need to set a value for the var, whatever it is
# Then it is considered -> SET 
# Why ?
# It testing within the cluster 
# If BASE - DOMAIN is not set, it wil not work
# The case in our case ;o) 
DAST_DISABLED  # Set to 1 

# Why ?
# NEEDS an additional feature to be set up in cluster in advance 
# See failed pipeline 
CLUSTER_IMAGE_SCANNING_DISABLED # Set to 1 

# After you have disabled it on the next run, the job
# will not be executed anymore 
# YOU WILL NOT SEE THE JOB IN THE PIPELINE 


```

## Setting a namespace for the project 

```
# Adjust to your tln - Nr 
Settings-> CI/CD -> Variables 

KUBE_NAMESPACE   tln1 



```

## Reference.

  * There is a whole list, what can be disabled
  * It might differ from gitlab to gitlab version
  * https://docs.gitlab.com/ee/topics/autodevops/customize.html#disable-jobs 
