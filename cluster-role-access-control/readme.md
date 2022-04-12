# Role
numune role.yml faylina esasendi. Role yalniz qeyd olunan namespaceye aiddir. Fayldada gorunduyu kimi deyisiklikler yalniz 'default' ns-de icra olunacaqdir:
1. apiGroups: [""]  --  bu hisse eger bos buraxilirsa demekki butun core api group-lara aid edilir.
2. resources: ["pods"]  --  teyin olunan role-da hansi resursdan istifade edecek odur, burda yalniz pods a aid nelerse ede bilecekdir
3. verbs: ["get", "watch", "list"]  --  bu ise yuxarida qeyd etdiyimiz obyekt yeniki pod uzerinde neleri list edecik odur, yeniki kubectl get pod ve s.
4. namespace: default  --  role yalniz default ns-e aid edilir.

# ClusterRole
ClusterRole ise Role-den ferqi budurki deyisiklikler butun klustere aid edilir. Numune clusterrole.yml faylindadir. Syntax-i demek olaraq Role ile eynidir:
1.   resources: ["secrets"]  --  yalniz burada deyisiklik ucun butun klusterde yalniz 'secrets' olaraq obyektinden istifade edeceyik.

# RoleBinding
Role yaratdir ve icazeler verdik, lakin bunlar hansi user-le uzerinde icra olunacaq? bunu RoleBinding ile edirik. Numune rolebinding.yml faylindadir:
1. subjects:  --  bu hissede RoleBinding-in hansi usere verilmesi yazilir, yeniki mzsamadov@xalqbank.az useri bu RoleBinding-den istifade edecekdir
- kind: User
  name: mzsamadov@xalqbank.az
  apiGroup: rbac.authorization.k8s.io
2. roleRef:  --  mzsamadov@xalqbank.az userinin hansi Role-a aid olacaqdir o gosterilir.
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: pod-reader

# ClusterRoleBinding
RoleBinding ile eynidir ve numune clusterrolebinding.yml faylindadir

Yuxarida gosterilen numunelere esasen 'mzsamadov-context' context-in altinda kecid edirik ve default ns-de podu list edirik. Gorunurki pod yoxdur ammaki list edir. 
```
# k config use-context mzsamadov-context
Switched to context "mzsamadov-context".
# k get pod
No resources found in default namespace.
```
Asagidaki komanda ile ise gorunurki butun klusterde secretleri list etmek olur:
```
# k get secret -A
NAMESPACE         NAME                                               TYPE                                  DATA   AGE
default           default-token-hhrt8                                kubernetes.io/service-account-token   3      7d23h
default           mysecret                                           Opaque                                3      8d
default           mysecret-to-file                                   Opaque                                3      7d23h
default           nginx-ingress-serviceaccount-token-hkx96           kubernetes.io/service-account-token   3      7d23h
istio-system      default-token-rdnfk                                kubernetes.io/service-account-token   3      50d
istio-system      dock                                               kubernetes.io/dockerconfigjson        1      50d
```