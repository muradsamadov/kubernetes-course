# DaemonSet
menasi budur ki, yaradilan podlar butun node-lar uzerinde calisir. Daha cox monitoring ve logging ucun istifade olunur. Teleb ola bilerki butun node-larin loglari yigilsin ve bu halda DaemonSet tetbiq etmekle pod butun node-lar uzerinde calisir. Asagidaki daemonset.yml misalinda logging numune kimi gosterilmisdir.
```
# k apply -f daemonset.yml
```
daemonset.yml faylin icini nezerden kecirek:
Qeyd etdikki daemonset-de yaradilan podlar butun node-lar uzerinde calisir ammaki master node-un ozunun tainiti varki buda defaul olaraq gelir, burada yalniz pod hemin taint ile uygun olmalidirki podda master node-lar uzerinde yaransin:
```
tolerations:
      - key: node-role.kubernetes.io/master:NoSchedule
        effect: NoSchedule

# k describe nodes  k8s-master01 |grep -i taint
Taints:             node-role.kubernetes.io/master:NoSchedule
```
Yuxaridaki halda master node-a mexsus taini tetbiq edirik.
Asagidanda gorunduyu kimi daemonset.yml faylini tetbiq eden zaman pod butun node-lar uzerinde running oldu:
```
# k get pod -o wide
NAME                 READY   STATUS    RESTARTS   AGE     IP             NODE           NOMINATED NODE   READINESS GATES
logdaemonset-8hzhk   1/1     Running   0          2m39s   10.244.3.153   k8s-worker01   <none>           <none>
logdaemonset-lhmdg   1/1     Running   0          61s     10.244.4.102   k8s-worker02   <none>           <none>
logdaemonset-qrd9t   1/1     Running   0          2m39s   10.244.1.2     k8s-master02   <none>           <none>
logdaemonset-xh8v2   1/1     Running   0          2m39s   10.244.2.3     k8s-master03   <none>           <none>
logdaemonset-z4tc7   1/1     Running   0          2m39s   10.244.0.3     k8s-master01   <none>           <none>

# k get daemonsets.apps  
NAME           DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
logdaemonset   5         5         5       5            5           <none>          2m54s
```