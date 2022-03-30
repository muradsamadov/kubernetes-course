# Namespace

Normalda ns tetbiq etmediyimiz halda butun prosesler default ns-de calisir. Eger isteyiremki butun komandalar cari olaraq basqa ns-de calissin (mes: metallb-system) onda bunu edirik:
```
# k config set-context --current --namespace=metallb-system
```
Bu halda '# k get pod' edende yalniz 'metallb-system' ns-de calisan podlari list edecekdir. Hal-hazirki contexte baxmaq ucun:
```
# k config get-contexts 
CURRENT   NAME                          CLUSTER      AUTHINFO           NAMESPACE
*         kubernetes-admin@kubernetes   kubernetes   kubernetes-admin   metallb-system
```
Yeniden bu komandani icra edib default ns-e qayitmaq olar:
```
# k config set-context --current --namespace=default
```