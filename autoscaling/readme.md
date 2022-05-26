Horizontal AutoScaling

Menasi odurki ola bilirki layihenin yuklu olan vaxti olur, ve yaradilan replikanin sayini HorizontalPodAutoscaler ile avtomatik artirmaq olur.
# k apply -f hpa-example.yml  --  parametrlerinin detaillari faylda etrafli qeyd olunub

Asagida hal-hazirda gorunduyu kimi deployment 0% yukludur, gerek yuk 50%i kecsinki autoscaling olsun. Bu halda serverden bu deploymente dayanmadan requestler gonderek ve yuk altina salaq servisi:
```[root@k8s-master01 autoscaling]# k get horizontalpodautoscalers.autoscaling 
NAME                     REFERENCE                TARGETS   MINPODS   MAXPODS   REPLICAS   AGE
hpa-example-autoscaler   Deployment/hpa-example   0%/50%    1         10        3          5m22s

[root@k8s-master01 autoscaling]# while true; do wget -q -O- http://192.168.52.250:31001; done
[root@k8s-master01 ~]# k get horizontalpodautoscalers.autoscaling
NAME                     REFERENCE                TARGETS   MINPODS   MAXPODS   REPLICAS   AGE
hpa-example-autoscaler   Deployment/hpa-example   59%/50%   1         10        3          8m
[root@k8s-master01 ~]# k get pod
NAME                           READY   STATUS              RESTARTS   AGE
hpa-example-776fdff64d-6cbgs   0/1     ContainerCreating   0          2s
hpa-example-776fdff64d-fqt8n   0/1     ContainerCreating   0          17s
hpa-example-776fdff64d-gnnxw   1/1     Running             0          8m3s
hpa-example-776fdff64d-kckjz   1/1     Running             0          8m3s
hpa-example-776fdff64d-mhdv2   0/1     ContainerCreating   0          2s
hpa-example-776fdff64d-tkvbx   0/1     ContainerCreating   0          2s
hpa-example-776fdff64d-vc2h8   0/1     ContainerCreating   0          2s
hpa-example-776fdff64d-vfr52   1/1     Running             0          8m3s
```

Yuxaridanda gorunduyu kimi ardicilq requestler gonderdik ve cpu 59%i oldugu ucun yeni podlari yaratdi. Eger cpu asagi dusse ozu podlari avtomatik olaraq silecekdir
