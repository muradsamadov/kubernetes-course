NodeAffinity

Asagidaki komanda ile node-larin label-na baxiriq:
```
[root@k8s-master01 affinity]# k get node --show-labels 
NAME           STATUS   ROLES                  AGE   VERSION   LABELS
k8s-master01   Ready    control-plane,master   30d   v1.23.0   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=k8s-master01,kubernetes.io/os=linux,node-role.kubernetes.io/control-plane=,node-role.kubernetes.io/master=,node.kubernetes.io/exclude-from-external-load-balancers=
k8s-node01     Ready    <none>                 30d   v1.23.0   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=k8s-node01,kubernetes.io/os=linux
k8s-node02     Ready    <none>                 30d   v1.23.0   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,hardware=high-spec,kubernetes.io/arch=amd64,kubernetes.io/hostname=k8s-node02,kubernetes.io/os=linux
```
Asagidaki gosterdiyim misalda Affinity teyin edirem, bu ise podun hansi node uzerinde up olacagini bildirir. Asagidaki misalda node-affinity.yaml faylini daha detalli baxaq:
```
    spec:
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution: # required olan yeniki buna pod buna aid olan key,value-de up olacaqdir ammaki eger preferred-de varsa ilk once preferedde up olacaqdir, eger preferred yoxdursa onda required key,value nedirse ona aid olan label-de up olacaqdir
            nodeSelectorTerms:
            - matchExpressions:
              - key: env
                operator: In
                values:
                - dev
          preferredDuringSchedulingIgnoredDuringExecution:
          - weight: 1
            preference:
              matchExpressions:
              - key: team
                operator: In
                values:
                - engineering-project1
```
Misal ucun butun node-larda key,valueni env=dev olaraq teyin edirik:
```
# k label nodes k8s-node02 env=dev
# k label nodes k8s-node01 env=dev
```
env=dev butun node-lara teyin edilenden sonra node-affinity.yaml faylini icra edende podlar her iki node uzerinde calisir. Amma men sonradan k8s-node01 node- asagidaki kimi label olaraq preferred etsem:
```
# k label nodes k8s-node01 team=engineering-project1
```
Yuxaridaki kimi etsem ve k8s-node02 de calisan hansisa podu delete etsem onda hemin pod k8s-node01 serveri uzerinde calisacaqdir cunki preferred olaraq team key-i bu node uzerindedir.
