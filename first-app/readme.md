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

# restartPolicy

3 dene restartPolicy novu movcuddur ve bu faylda 'pod-lifecycle.yml' qeyd edecem:
```
restartPolicy: Always  --  yeniki pod ne vaxt ki isini bitirir hemise restart getsin
restartPolicy: Never  --  pod isini tamamlayandan sonra hec vaxt restart getmesin
restartPolicy: OnFailure  --  pod yaranan zaman fail oldug halda restart getsin, yeniki pod normal running olarsa ve isini bitirerse restart getmesin
```
---

# Pod Multicontainer

Best-practise-de pod-multiconainer istifade edilmir. Yalniz o zaman istifade edilirki her podun monitorinqi ola bilerki (mes: fluentd) bu halda istifade oluna biler. pod-multicontainer.yml faylinda qeyd olunubki sidecarcontainer containeri publikden kodu cekir ve webcontainer containerinde ise calisdirir. Qeyd edimki pod-multiconainerde ipler ve volume eyni olur, ve bir localhost daxilinde eyni containermis kimi calisirlar.