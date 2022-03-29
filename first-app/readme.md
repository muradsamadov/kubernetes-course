```
k apply -f first-app/helloworld.yml  --  nodehelloworld.example.com podu run edirik
k apply -f first-app/helloworld-nodeport-service.yml  --  helloworld-service servisi run edirik. Bu halda NodePort type olaraq calisdiririq, yeniki podu cole 31001 portu altinda cixaririq:
```
Asagidaki kimi :
```
[root@k8s-master01 kubernetes-course]# k get svc
NAME                 TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)           AGE
helloworld-service   NodePort    10.102.217.46   <none>        31001:31001/TCP   1s
```

Yuxarida qeyd olunan 31001:31001/TCP ikinci "31001" portu NodePort portudur, birinci ise cluster daxilindi istifade olunan portdur. Colden NodePort ile podu cagira bilerik:
```
curl  192.168.52.250:31001
Hello World!
```

---

