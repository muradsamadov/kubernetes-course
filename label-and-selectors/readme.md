# Labels examples

misallari podlabel.yml faylina gore icra edeceyik. Ilk novbede podlabel.yml faylini apply edirik^
```
# k apply -f podlabel.yml
```
Asagidaki komanda ile 'tier' keyine malik olan podlari select edirik:
```
# k get pod -l 'tier' --show-labels 
NAME   READY   STATUS    RESTARTS   AGE   LABELS
pod1   1/1     Running   0          59m   app=firstapp,tier=frontend
pod2   1/1     Running   0          59m   app=firstapp,tier=frontend
pod3   1/1     Running   0          59m   app=firstapp,tier=backend
```
Bu komanda ile tier=backend labela malik olan podlari list edirik:
```
# k get pod -l 'tier=backend' --show-labels 
NAME   READY   STATUS    RESTARTS   AGE   LABELS
pod3   1/1     Running   0          33s   app=firstapp,tier=backend
```
Bu komanda ile app=firstapp,tier=frontend labela malik olan podlari list edirik:
```
# k get pod -l 'app=firstapp,tier=frontend' --show-labels 
NAME   READY   STATUS    RESTARTS   AGE   LABELS
pod1   1/1     Running   0          93s   app=firstapp,tier=frontend
pod2   1/1     Running   0          93s   app=firstapp,tier=frontend
```

---
# Selector examples

podselector.yml faylindaki misalda podun yalniz 'hddtype: ssd' bu label-i olan node-da calisacagini gostermekdir. nodeSelector ile podun hansi node uzerinde calisacagini gostermekdir. Asagidaki komanda ile k8s-worker02 node-na 'hddtype=ssd' label-ni teyin edirik:
```
# k label nodes  k8s-worker02 hddtype=ssd
# k get node -l 'hddtype' --show-labels 
NAME           STATUS   ROLES    AGE   VERSION   LABELS
k8s-worker02   Ready    <none>   41d   v1.23.3   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,hddtype=ssd,kubernetes.io/arch=amd64,kubernetes.io/hostname=k8s-worker02,kubernetes.io/os=linux
```
Sonra ise podselector.yml faylini apply ederken gorunurki pod k8s-worker02 node-u uzerinde calisir:
```
# k get pod -o wide
NAME    READY   STATUS    RESTARTS   AGE     IP            NODE           NOMINATED NODE   READINESS GATES
pod11   1/1     Running   0          2m48s   10.244.4.53   k8s-worker02   <none>           <none>
```