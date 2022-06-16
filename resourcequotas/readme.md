# ResourceQuota

In this capture we talk about ResourceQuota. I explain it in examples. Lets look this file 'resourcequota.yml':
```
# cat resourcequota.yml
apiVersion: v1
kind: Namespace
metadata:
  name: myspace
---
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-quota
  namespace: myspace
spec:
  hard:
    requests.cpu: "1"
    requests.memory: 1Gi
    limits.cpu: "2"
    limits.memory: 2Gi
---
apiVersion: v1
kind: ResourceQuota
metadata:
  name: object-quota
  namespace: myspace
spec:
  hard:
    configmaps: "10"
    persistentvolumeclaims: "4"
    replicationcontrollers: "20"
    secrets: "10"
    services: "10"
    services.loadbalancers: "2"
```
Above we create 'myspace' ns and add this ns quota. In 'compute-quota' name of quota i set only requirements and 'object-quota' quota i set only object limits. When i create this no quota deployment file 'helloworld-no-quotas.yml' this is deployment not be run  because when you are create quota with hard limit you must set quota with limits:
```
# k apply -f helloworld-no-quotas.yml
# k describe rs helloworld-deployment-885c748bf
Name:           helloworld-deployment-885c748bf
Namespace:      myspace
Selector:       app=helloworld,pod-template-hash=885c748bf
Labels:         app=helloworld
                pod-template-hash=885c748bf
Annotations:    deployment.kubernetes.io/desired-replicas: 3
                deployment.kubernetes.io/max-replicas: 4
                deployment.kubernetes.io/revision: 1
Controlled By:  Deployment/helloworld-deployment
Replicas:       0 current / 3 desired
Pods Status:    0 Running / 0 Waiting / 0 Succeeded / 0 Failed
Pod Template:
  Labels:  app=helloworld
           pod-template-hash=885c748bf
  Containers:
   k8s-demo:
    Image:        quay.io/muradsamadov/pythonapp:v1
    Port:         5000/TCP
    Host Port:    0/TCP
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type             Status  Reason
  ----             ------  ------
  ReplicaFailure   True    FailedCreate
Events:
  Type     Reason        Age                 From                   Message
  ----     ------        ----                ----                   -------
  Warning  FailedCreate  111s                replicaset-controller  Error creating: pods "helloworld-deployment-885c748bf-6s9qt" is forbidden: failed quota: compute-quota: must specify limits.cpu,limits.memory,requests.cpu,requests.memory
  Warning  FailedCreate  111s                replicaset-controller  Error creating: pods "helloworld-deployment-885c748bf-25pms" is forbidden: failed quota: compute-quota: must specify limits.cpu,limits.memory,requests.cpu,requests.memory
  Warning  FailedCreate  111s                replicaset-controller  Error creating: pods "helloworld-deployment-885c748bf-gtl7k" is forbidden: failed quota: compute-quota: must specify limits.cpu,limits.memory,requests.cpu,requests.memory
  Warning  FailedCreate  111s                replicaset-controller  Error creating: pods "helloworld-deployment-885c748bf-wtvgf" is forbidden: failed quota: compute-quota: must specify limits.cpu,limits.memory,requests.cpu,requests.memory
  Warning  FailedCreate  111s                replicaset-controller  Error creating: pods "helloworld-deployment-885c748bf-6n4kb" is forbidden: failed quota: compute-quota: must specify limits.cpu,limits.memory,requests.cpu,requests.memory
  Warning  FailedCreate  111s                replicaset-controller  Error creating: pods "helloworld-deployment-885c748bf-s2hg6" is forbidden: failed quota: compute-quota: must specify limits.cpu,limits.memory,requests.cpu,requests.memory
  Warning  FailedCreate  111s                replicaset-controller  Error creating: pods "helloworld-deployment-885c748bf-pfl6f" is forbidden: failed quota: compute-quota: must specify limits.cpu,limits.memory,requests.cpu,requests.memory
  Warning  FailedCreate  110s                replicaset-controller  Error creating: pods "helloworld-deployment-885c748bf-4mdcv" is forbidden: failed quota: compute-quota: must specify limits.cpu,limits.memory,requests.cpu,requests.memory
  Warning  FailedCreate  110s                replicaset-controller  Error creating: pods "helloworld-deployment-885c748bf-7mjfz" is forbidden: failed quota: compute-quota: must specify limits.cpu,limits.memory,requests.cpu,requests.memory
  Warning  FailedCreate  29s (x6 over 108s)  replicaset-controller  (combined from similar events): Error creating: pods "helloworld-deployment-885c748bf-x767l" is forbidden: failed quota: compute-quota: must specify limits.cpu,limits.memory,requests.cpu,requests.memory
```
Above i look with no quota deployment not deploy. Below i set 'helloworld-with-quotas.yml' deployment with quota:
```
# k apply -f helloworld-with-quotas.yml
# k get pod
NAME                                     READY   STATUS    RESTARTS   AGE
helloworld-deployment-54d5b9f7c4-9tdx7   1/1     Running   0          3s
helloworld-deployment-54d5b9f7c4-zktsf   1/1     Running   0          3s
# k describe rs helloworld-deployment-54d5b9f7c4
Name:           helloworld-deployment-54d5b9f7c4
Namespace:      myspace
Selector:       app=helloworld,pod-template-hash=54d5b9f7c4
Labels:         app=helloworld
                pod-template-hash=54d5b9f7c4
Annotations:    deployment.kubernetes.io/desired-replicas: 3
                deployment.kubernetes.io/max-replicas: 4
                deployment.kubernetes.io/revision: 1
Controlled By:  Deployment/helloworld-deployment
Replicas:       2 current / 3 desired
Pods Status:    2 Running / 0 Waiting / 0 Succeeded / 0 Failed
Pod Template:
  Labels:  app=helloworld
           pod-template-hash=54d5b9f7c4
  Containers:
   k8s-demo:
    Image:      quay.io/muradsamadov/pythonapp:v1
    Port:       5000/TCP
    Host Port:  0/TCP
    Limits:
      cpu:     400m
      memory:  1Gi
    Requests:
      cpu:        200m
      memory:     512Mi
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type             Status  Reason
  ----             ------  ------
  ReplicaFailure   True    FailedCreate
Events:
  Type     Reason            Age                From                   Message
  ----     ------            ----               ----                   -------
  Normal   SuccessfulCreate  31s                replicaset-controller  Created pod: helloworld-deployment-54d5b9f7c4-zktsf
  Warning  FailedCreate      31s                replicaset-controller  Error creating: pods "helloworld-deployment-54d5b9f7c4-6xqhw" is forbidden: exceeded quota: compute-quota, requested: limits.memory=1Gi,requests.memory=512Mi, used: limits.memory=2Gi,requests.memory=1Gi, limited: limits.memory=2Gi,requests.memory=1Gi
  Normal   SuccessfulCreate  31s                replicaset-controller  Created pod: helloworld-deployment-54d5b9f7c4-9tdx7
  Warning  FailedCreate      31s                replicaset-controller  Error creating: pods "helloworld-deployment-54d5b9f7c4-w7zzt" is forbidden: exceeded quota: compute-quota, requested: limits.memory=1Gi,requests.memory=512Mi, used: limits.memory=2Gi,requests.memory=1Gi, limited: limits.memory=2Gi,requests.memory=1Gi
  Warning  FailedCreate      31s                replicaset-controller  Error creating: pods "helloworld-deployment-54d5b9f7c4-xvnxm" is forbidden: exceeded quota: compute-quota, requested: limits.memory=1Gi,requests.memory=512Mi, used: limits.memory=2Gi,requests.memory=1Gi, limited: limits.memory=2Gi,requests.memory=1Gi
  Warning  FailedCreate      31s                replicaset-controller  Error creating: pods "helloworld-deployment-54d5b9f7c4-8mw6c" is forbidden: exceeded quota: compute-quota, requested: limits.memory=1Gi,requests.memory=512Mi, used: limits.memory=2Gi,requests.memory=1Gi, limited: limits.memory=2Gi,requests.memory=1Gi
  Warning  FailedCreate      31s                replicaset-controller  Error creating: pods "helloworld-deployment-54d5b9f7c4-h89v5" is forbidden: exceeded quota: compute-quota, requested: limits.memory=1Gi,requests.memory=512Mi, used: limits.memory=2Gi,requests.memory=1Gi, limited: limits.memory=2Gi,requests.memory=1Gi
  Warning  FailedCreate      31s                replicaset-controller  Error creating: pods "helloworld-deployment-54d5b9f7c4-gxpkq" is forbidden: exceeded quota: compute-quota, requested: limits.memory=1Gi,requests.memory=512Mi, used: limits.memory=2Gi,requests.memory=1Gi, limited: limits.memory=2Gi,requests.memory=1Gi
  Warning  FailedCreate      31s                replicaset-controller  Error creating: pods "helloworld-deployment-54d5b9f7c4-x9c7k" is forbidden: exceeded quota: compute-quota, requested: limits.memory=1Gi,requests.memory=512Mi, used: limits.memory=2Gi,requests.memory=1Gi, limited: limits.memory=2Gi,requests.memory=1Gi
  Warning  FailedCreate      31s                replicaset-controller  Error creating: pods "helloworld-deployment-54d5b9f7c4-tdx8b" is forbidden: exceeded quota: compute-quota, requested: limits.memory=1Gi,requests.memory=512Mi, used: limits.memory=2Gi,requests.memory=1Gi, limited: limits.memory=2Gi,requests.memory=1Gi
  Warning  FailedCreate      30s                replicaset-controller  Error creating: pods "helloworld-deployment-54d5b9f7c4-sfx22" is forbidden: exceeded quota: compute-quota, requested: limits.memory=1Gi,requests.memory=512Mi, used: limits.memory=2Gi,requests.memory=1Gi, limited: limits.memory=2Gi,requests.memory=1Gi
  Warning  FailedCreate      29s (x5 over 30s)  replicaset-controller  (combined from similar events): Error creating: pods "helloworld-deployment-54d5b9f7c4-dtjgj" is forbidden: exceeded quota: compute-quota, requested: limits.memory=1Gi,requests.memory=512Mi, used: limits.memory=2Gi,requests.memory=1Gi, limited: limits.memory=2Gi,requests.memory=1Gi
```
Above the example i set 3 replicas deplotment and running only 2 pod. I describe rs and i see error logs. This error i this quota run only max memory 2GB ram. We are set 3 '1' GB. Thats why this third pod not creating.

# LimitRange
