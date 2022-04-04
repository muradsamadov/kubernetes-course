# NodeAffinity
Menasi odur ki, yaranan podu hansi node uzerinde islemesidir. podnodeaffinity.yml faylinda misal olaraq gosterilib:
1. nodeaffinitypod1 podunda 'requiredDuringSchedulingIgnoredDuringExecution' parametrinin menasi budur ki, pod yalniz labeli app=blue olan node uzerinde islemelidir. Yeni bundan basqa labl olan node uzerinde calisdirila bilmez. 'NotIn' ise onun eksidir. Yeni labeli app=blue olan node uzerinde calismasin. Eger bele bir label yoxdursa hecbir node uzerinde calismasin demekdir. Artiq biz buna antiaffinity deyirik. 'Exists' ise yalniz labeli 'app' olan node-u axtarir ve value-na fikir vermir. 'NotExist' ise tam eksine, labeli 'app' olamayan node-u axtarir ve pod orada calisir.
2. nodeaffinity2 podunda 'preferredDuringSchedulingIgnoredDuringExecution' parametrinin menasi app=blue ve ya app=red labeli olan node uzerinde pod calisir. Eger bele bir labeli olan node tapmirsa yenede diger node-uzerinde pod calisir. Yeniki hec bir halda pod qiraqda qalmir. Her iki halda pod hansisa node uzerinde calismis olur. Burada parametr olan 'weight' ise hansinin weight-i boyukdurse ilk novbede hemin pod calisir daha sonra ise deyeri kicin olan pod node uzerinde calismis olur.

Qeyd: her iki parametrlelrin sonunda 'IgnoredDuringExecution' hissesinin menasi budur ki, hansisa halda qeyd olunan label node uzerinde silinerse ve hemin vaxti pod hemin node uzerinde cari olaraq calisirsa bu halda podu node userinden silmir. Bu parametrin menasi budur.

Asagidaki komanda vasitesile noda label set edirik, ikinci komanda ile ise silirik:
```
# k label  node k8s-worker02 app=blue
# k label  node k8s-worker02 app-
```