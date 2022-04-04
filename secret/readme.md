# Secrets
secret.yml faylinda sade stringData formatinda aciq data yaradildi:
```
# k apply -f secret.yml
# k describe secrets mysecret 
Name:         mysecret
Namespace:    default
Labels:       <none>
Annotations:  <none>

Type:  Opaque

Data
====
db_server:    14 bytes
db_username:  5 bytes
db_password:  8 bytes
```
Daha bir numune ucun asagida secret-i manual olaraq yaradiram:
```
# k create secret generic mysecret-to-file --from-literal=db_server=server.txt --from-literal=db_username=username.txt --from-literal=db_password=passw.txt
# k describe secrets mysecret-to-file 
Name:         mysecret-to-file
Namespace:    default
Labels:       <none>
Annotations:  <none>

Type:  Opaque

Data
====
db_server:    10 bytes
db_username:  12 bytes
db_password:  9 bytes
```
numuneni podsecret.yml faylinda edirem:
```
# k apply -f podsecret.yml
```
secretpodvolume adindaki podda yaradilan "mysecret-to-file" secreti fayl olaraq containerde /secret direktoriyasina atiram. Hemin /secret/db_username, /secret/db_server, /secret/db_password fayllarinda ENV kimi istifade etmek olar.
secretpodenv bu halda ise env-ni secret obyektinde oxutdururuq. Bu halda ise podda yalniz qeyd olunan key-leri oxuyur.
secretpodenvall bu halda ise "mysecret-to-file" secretinde ne ENV varsa hamisini set edir.









env kimi istifade olunur yalniz sifrelenmis formada:
```
$ k apply -f helloworld-secrets.yml  --  db-secrets adinda secret yaradiriq ve username ve parol teyin edirik, username ve parol base64 formatinda sifrelenmisdir
$ echo -n 'root' | base64  -- ile 'root' sozunu base64 formatinda sifreleyirik ve outputda verileni username kimi secret faylina elave edirik.
$ k apply- f helloworld-secrets-volumes.yml  --  helloworld-deployment adinda deploymet yaradiriq ve ENV kimi secretden istifade edirik. Burada /etc/creds direktoriyasinda lazim olan username ve parolu elde etmek olar
```
kubernetes-course/wordpress proyektinde secrete aid misal verilib. Wordpress qurularken istifade olunan WORDPRESS_DB_PASSWORD, WORDPRESS_DB_HOST, MYSQL_ROOT_PASSWORD ENV-lar secret faylinda istifade olunaraq qurulub
