# Walkthrough - Installation (Helm) and Setup 

## Prerequisites

  * Ubuntu 20.04 with running microk8s single cluster 
  * Works on any other cluster, but installing helm is different 

## Prepare 

```
# Be sure helm is installed on your client 
# In our walkthrough, we will do it directly on 1 node, 
# which is not recommended for Production 

```

## Walkthrough 

### Step 1: install helm, if not there yet

```
snap install --classic helm 
```

### Step 2: Rollout prometheus/grafana stack in namespace prometheus 

```
# add prometheus repo 

```

## Reference:

  * Techworld with Nana: [https://www.youtube.com/watch?v=QoDqxm7ybLc](https://youtu.be/QoDqxm7ybLc?t=190)
