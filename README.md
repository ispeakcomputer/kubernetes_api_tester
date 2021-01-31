# Kubernetes API Tester For Intermittent Kubectl Failures on HA Master Cluster.

## Two different scripts, two different ways to run.

1. This will parse out the kube masters IPs from Vaqureo config and then test each masters kube-apiserver API individually 
**or**
2. This will parse out masters from a file you feed the script to test.

## Tests Timeouts and Escalation Failures

This script tries to return pods with 'kubectl get pods' because just returning nodes with 'kubectl get nodes' doesn't account for the API's escalation failures. We have seen both issues in the wild with kube-apiserver in earlier versions of Kubernetes. 

Checking pods with 'get pods' covers the two different ways I have seen the API fail. Getting pods resources will test the auth function of the service as well as timeouts / etc.

### How to run

This script is for testing the Master APIserver **behind** the load balancer in a HA masters Kubernetes cluster

***./api_tester.sh ~/vaquero-inventory/sites//site-file/hosts-coreos-denver-example.yml***

If the output gets stuck on one node and then times out then that nodes kube-apiserver process is causing issues. jump into that machine and run 

***'sudo systemctl restart kube-apiserver'***

***Output (Timeout Error Could Vary)***
```>$./api_tester.sh ~/vaquero-inventory/sites//smf-ingest/hosts-coreos-smf.yml
You wanna search for masters in host file  /Users/rwrgh667/vaquero/sites//ingest/hosts-smf.yml
working on 10.144.125.5
**Showing just 5 Pods Only**
smf       smf-lp-cache-1                           4/4     Running   0          20d
smf       smf-lp-cache-2                           4/4     Running   0          20d
smf       smf-lp-cache-3                           4/4     Running   0          20d
smf       smf-lp-cache-4                           4/4     Running   0          20d
smf       tiller--59dcd9644c-g2qbw                       1/1     Running   0          20d
working on 10.144.125.6
**Showing just 5 Pods Only**
smf       smf-dash-lp-cache-1                           4/4     Running   0          20d
smf       smf-dash-lp-cache-2                           4/4     Running   0          20d
smf       smf-dash-lp-cache-3                           4/4     Running   0          20d
smf       smf-dash-lp-cache-4                           4/4     Running   0          20d
smf       tiller--59dcd9644c-g2qbw                       1/1     Running   0          20d
working on 10.144.125.7
**Showing just 5 Pods Only**
ERROR: API Timeout
```
 
 
