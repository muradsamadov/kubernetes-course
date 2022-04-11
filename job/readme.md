# Job
asagidaki butun misallar job.yml faylina aiddir:
1. parallelism: 2  --  yaranan podlarin nece nece yaranmasidir, mes 10 pod yaranirsa bunu 2-2 yaratsin.
2. completions: 10  --  bu jobda nece dene pod yaratmagimizdir.
3. backoffLimit: 5  --  toplam olaraq 5 defe xeta gelerse bu job isini dayandir.
4. activeDeadLineSeconds: 100  --  eger bu task 100saniye icinde tamamlanmazsa onda bu jobu dayandir.
Yucarida qeyd olunan 'parallelism' ve 'completions' teyin etmesek default olaraq 1 deyerini alirlar.
```
# k apply -f job.yml
# k get pod
NAME       READY   STATUS      RESTARTS   AGE
v1-47ntp   0/1     Completed   0          27s
v1-4gbvd   0/1     Completed   0          50s
v1-9hm2n   0/1     Completed   0          53s
v1-ctrlq   0/1     Completed   0          31s
v1-gs5n6   0/1     Completed   0          63s
v1-hxn88   0/1     Completed   0          19s
v1-sdn98   0/1     Completed   0          63s
v1-wjr4g   0/1     Completed   0          38s
v1-wtstx   0/1     Completed   0          41s
v1-zfj86   0/1     Completed   0          16s
```
Yucaridanda gorunduyu kimi 2-2 podlar 10-kimi yarandi ve hamisi 'Completed' oldu. Bu halda podlar silinmir sadece olaraq sonulu veziyyetde tamamlanmis kimi qalir.