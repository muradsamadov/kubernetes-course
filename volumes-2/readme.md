# Persistent Volume
numuneler pv.yml faylinda gosterilmisdir. Asagida gosterilmis capacity bolmesinde ne qeder volume yaratmagimizi bildirir. Numunede de gorunduyu kimi 1Gi hecminde volume yaradilib:
```
spec:
  capacity:
    storage: 1Gi
```
Asagida gosterilen accessModes-in 3 novu vardir:
1. ReadWriteOnce - bu volume yalniz tek bir poda baglana biler ve hem read hemde write permission-u olacaqdir
2. ReadOnlyMany - bu volume birden artiq poda baglana biler lakin yalniz read permission-u olacaqdir
3. ReadWriteMany - bu volume ise birden artiq poda baglana biler ve hem read hemde write permission-u olacaqdir
```
  accessModes:
    - ReadWriteOnce
```
Asagida gosterilen   persistentVolumeReclaimPolicy-in 3 novu vardir:
1. persistentVolumeReclaimPolicy: Retain - pv islenilib bitdikden sonra oldugu kimi qalir ve burdaki datalari biz basqa birde koplayib istifade ede bilerik
2. persistentVolumeReclaimPolicy: Recycle - pv ile is bitdikden sonra pv silinmir lakin datalar silinir
3. persistentVolumeReclaimPolicy: Delete - bu halda ise pv ile is bitdikden sonra pv ve datalar silir

# Persistent Volume Claim
pv ni poda baglamaq ucun mutleq pvc yaradilmalidir ve butun numuneler pvc.yml faylinda qeyd olunub:

deployment.yml misalinda mysql yaradiriq ve pv.yml-da qeyd olundugu kimi /localstorage direktoriyasi yaradilir ve datalar ora yazilir. pod hansi serverde yaranirsa /localstorage-de orada yaranir. Tekrar deploymenti silib yeniden hemin mysql podu yaradanda ise kohne data oldugu kimi qalmis olur.