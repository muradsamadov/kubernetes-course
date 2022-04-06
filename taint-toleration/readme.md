# Taint and Toleration
T&T-nin Affinity ile oxsarligi vardir ammaki affinitynin menasi budurki bu pod yalniz bu node uzerinde calismalidir. T&T-de ise bu node-larda buna uygun pod-lar calismalidir.  Misallara baxaq:
```
# k describe nodes  k8s-worker02 |grep -i Taint
Taints:             <none>
```
Yuxaridan gorunduyu kimi k8s-worker02 node-un Taini yixdur. Asagidaki komanda ile k8s-worker02 node-na taint elave edirik:
```
# k taint node  k8s-worker02 platform=production:NoSchedule
node/k8s-worker02 tainted
# k describe nodes  k8s-worker02 |grep -i Taint
Taints:             platform=production:NoSchedule
```
Yuxarida k8s-worker02 node-na platform=production:NoSchedule tainti set etdik. 'NoSchedule' menasi budur ki, eger node-a bu tetbiq olunubsa ve pod-a tetbiq olunmayibsa demek hemin pod bu node uzerinde calisa bilmez. Eks halda poda 'platform=production:NoSchedule' teqbiq olunubsa bu pod hemin node uzerinde calisacaqdir bsaqa hec bir halda pod diger node uzerinde calisa bilmez.
Asagidaki komanda ile tainiti k8s-worker02 nede uzerinde silirik:
```
# k taint node k8s-worker02 platform-
node/k8s-worker02 untainted
# k describe nodes k8s-worker02 |grep -i taint
Taints:             <none>
```

Indi ise bu tainti yeniden set edek cunki numunemizde bize lazim olacaqdir:
```
# k taint node  k8s-worker02 platform=production:NoSchedule
```
Asagidaki misalda gorunduyu kimi deploymente toleration tetbiq olunmadigina gore butun podlar 'k8s-worker01' node uzerinde calisir:
```
# k create deployment mydeployment --image=quay.io/muradsamadov/nginx --replicas=5
deployment.apps/mydeployment created
# k get pod  -o wide 
NAME                           READY   STATUS    RESTARTS   AGE    IP             NODE           NOMINATED NODE   READINESS GATES
mydeployment-cbfb44657-8jwmk   1/1     Running   0          113s   10.244.3.107   k8s-worker01   <none>           <none>
mydeployment-cbfb44657-gmfpm   1/1     Running   0          113s   10.244.3.108   k8s-worker01   <none>           <none>
mydeployment-cbfb44657-mpvql   1/1     Running   0          113s   10.244.3.104   k8s-worker01   <none>           <none>
mydeployment-cbfb44657-ms966   1/1     Running   0          113s   10.244.3.105   k8s-worker01   <none>           <none>
mydeployment-cbfb44657-pgjsr   1/1     Running   0          113s   10.244.3.106   k8s-worker01   <none>           <none>
```
