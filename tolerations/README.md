# Taint and Tolerations

# Taint a node
```
kubectl taint nodes NODE-NAME type=specialnode:NoSchedule
```

# Taint a node with NoExecute
```
kubectl taint nodes NODE-NAME testkey=testvalue:NoExecute
```
Yuxaridaki misallarda node-a taint teyin edirsen, tolerations ise pod hissede yazmalisan. tolerations.yaml faylinda tolerations qeyd olunub. Bunun 3 novu var: NoSchedule PreferNoSchedule ve NoExecute. Hal-hazirki misalda 'NoSchedule' icra edecem. Bu o demekdir ki, yaranan podlar hemde bu taint olan node uzerinde de up olsun.
# k taint node  k8s-node02 type=specialnode:NoSchedule
[root@k8s-master01 tolerations]# k describe nodes k8s-node02 | grep -i taint
Taints:             type=specialnode:NoSchedule

Yuxaridaki iki misalda node-a taint teyin edirik ve asagidaki tolerations.yml faylini icra edirik:
# k apply -f tolerations.yaml 

Faylda da gorunduyu kimi iki deployment qeyd olunub ammaki muqayise ucun yalniz birinde toleration tetbiq edilib. Icra olunduqdan sonra tolerations-1 deploymenti yalniz  'k8s-node02' node-da calisacaqdir cunki ora taint teyin edilmeyib ammaki tolerations-2 her iki node uzerinde de calisacaqdir bu halda toleration tetbiq olunmusdur.