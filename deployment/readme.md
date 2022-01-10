# Deployment

# k apply -f helloworld.yml  -- helloworld-deployment adinda deployment yaradiriq
# k rollout status deployment helloworld-deployment  --  helloworld-deployment deploymentin statusna baxiriq ki, pod deploy oldu ya yox
# k rollout history deployment helloworld-deployment  -- helloworld-deployment deploymentin revision nomrelerine yeniki hal-hazirda hansi veziyyetde deploy olunub ona baxa bilirik. Ne vaxtsa rollout undo etsek yenede buradan baxa bilirik. Misalcun asagidaki kimi:

Hal-hazirda app 1 defe deploy edilib:
# [root@k8s-master01 ~]# k rollout history deployment helloworld-deployment
deployment.apps/helloworld-deployment
REVISION  CHANGE-CAUSE
1         <none>

Yeni image-e deploy etsem misalcun buna wardviaene/k8s-demo:2m, bu zaman deployment 2 dene revision nomre olaraq deyisiklecekdir :
[root@k8s-master01 ~]# k rollout history deployment helloworld-deployment
deployment.apps/helloworld-deployment
REVISION  CHANGE-CAUSE
1         <none>
2         <none>

En sonuncu reqem hal-hazirda calisan deploymenin revision nomresidir, yeniki "2". Asagidaki komanda ile revision nomresi "1" olan veziyyete qayitmaq olar. Ola bilerki deploymentde nese bug var, bu halda rahat evvelki versiyaya qayitmaq olar:
[root@k8s-master01 ~]# k rollout undo deployment helloworld-deployment


---

# NodeSelector

# k apply -f helloworld-nodeselector.yml  --  helloworld-deployment deployment run edirik ve nodeSelector optionu ile podun hansi node uzerinde run olmasini secirik. Bu halda lable-den istifade edirik. Bu halda nodun labeli bu olmalidir:
nodeSelector:
  hardware: high-spec

Asagidaki komanda ile node-larin labellerine baxiriq ve "hardware=high-spec" adda label gormuruk:
[root@k8s-master01 deployment]# k get nodes --show-labels 
NAME           STATUS   ROLES                  AGE   VERSION   LABELS
k8s-master01   Ready    control-plane,master   25d   v1.23.0   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=k8s-master01,kubernetes.io/os=linux,node-role.kubernetes.io/control-plane=,node-role.kubernetes.io/master=,node.kubernetes.io/exclude-from-external-load-balancers=
k8s-node01     Ready    <none>                 25d   v1.23.0   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=k8s-node01,kubernetes.io/os=linux
k8s-node02     Ready    <none>                 25d   v1.23.0   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=k8s-node02,kubernetes.io/os=linux

Asagidaki komanda ile k8s-node2 servere label teyin edirik:
# k label nodes k8s-node02 hardware=high-spec
# k get nodes --show-labels -- ederek baxmaq olar

Bu  halda helloworld-deployment deploymenti up olacaqdir.

---

Liveness and Readiness Probe

# k apply -f helloworld-healthcheck.yml  -- helloworld-deployment deploymenti calisdiririq. Etrafli faylda qeyd olunub.

Liveness odur ki, misalcun bizim misaldakinda poda http GET sorgusu gonderir eger ok-dursa pod Running olur. Eks halda podu restart edecek.
Readiness-de ise pod yaranir ve podun islemeye hazir olub olmamagini bildirir. Bizim asagidaki misalda birinci liveness edir http GET cavabi qayidirsa sonra readiness edir. Eger bu haldada pod hemin GET-e cavab qayatrirsa demekki islemeye hazir veziyyetdedir:
# k apply -f helloworld-liveness-readiness.yml

---

Secrets

env kimi istifade olunur yalniz sifrelenmis formada:
# k apply -f helloworld-secrets.yml  --  db-secrets adinda secret yaradiriq ve username ve parol teyin edirik, username ve parol base64 formatinda sifrelenmisdir
# echo -n 'root' | base64  -- ile 'root' sozunu base64 formatinda sifreleyirik ve outputda verileni username kimi secret faylina elave edirik.
# k apply- f helloworld-secrets-volumes.yml  --  helloworld-deployment adinda deploymet yaradiriq ve ENV kimi secretden istifade edirik. Burada /etc/creds direktoriyasinda lazim olan username ve parolu elde etmek olar

kubernetes-course/wordpress proyektinde secrete aid misal verilib. Wordpress qurularken istifade olunan WORDPRESS_DB_PASSWORD, WORDPRESS_DB_HOST, MYSQL_ROOT_PASSWORD ENV-lar secret faylinda istifade olunaraq qurulub