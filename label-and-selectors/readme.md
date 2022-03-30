# Labels examples

misallari podlabel.yml faylina gore icra edeceyik. Ilk novbede podlabel.yml faylini apply edirik^
```
k apply -f podlabel.yml
```
Asagidaki komanda ile 'tier' keyine malik olan podlari select edirik:
```
# k get pod -l 'tier' --show-labels 
NAME   READY   STATUS    RESTARTS   AGE   LABELS
pod1   1/1     Running   0          59m   app=firstapp,tier=frontend
pod2   1/1     Running   0          59m   app=firstapp,tier=frontend
pod3   1/1     Running   0          59m   app=firstapp,tier=backend
```
