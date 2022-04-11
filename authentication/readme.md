# Authentication
Misalcun bir firmada developer olaraq ise basladiq ve developer kubernetes clustere qosulmalidir. Bu halda telebe esasen developer private.key ve csr fayli yaratmali ve kubernetes admine gondermelidir. Kubernetes adminde certificate authority-de onu imzalayaraq bir sertifikat yaradacaqdir. Developerde bu sertifikat vasitesile klustere qosula bilecekdir.
Ilk novbede developer certificate.key ve certificate.csr fayllari yaradacaqdir:
```
# openssl genrsa -out certificate.key 2048
# openssl req -new -key certificate.key -out certificate.csr -subj "/CN=mzsamadov@xalqbank.az/O=DevTeam"
```
Yuxaridaki key ve csr fayllarini yarandandan sonra kubernetes admine gonderirik. Adminde csr faylinda istifade ederek sertifikat yaradir ve developere gonderirik:
```
cat << EOF | kubectl apply -f -
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: certificate
spec:
  groups:
  - system:authenticated
  request: $(cat certificate.csr | base64 | tr -d "\n")
  signerName: kubernetes.io/kube-apiserver-client
  usages:
  - client auth
EOF
```
Yuxaridaki kimi CertificateSigningRequest yaradir ve onu approve edib developere gonderirik:
```
# k certificate approve certificate
# k get csr
NAME          AGE   SIGNERNAME                            REQUESTOR          REQUESTEDDURATION   CONDITION
certificate   18s   kubernetes.io/kube-apiserver-client   kubernetes-admin   <none>              Approved,Issued
```
'Approved,Issued' den gorunduyu kimi sertifikat approve olunub. Hemin bu yaranan sertifikati decode edib developere asagidaki komanda ile gonderirik:
```
# k get csr certificate -o jsonpath='{.status.certificate}' | base64 -d >> certificate.crt
```
Developerde hemin useri set edir:
```
# k config set-credentials mzsamadov@xalqbank.az --client-certificate=certificate.crt --client-key=certificate.key
```
Asagidaki komanda ile context yaradiriq ve hemin contexte kubernetes klusterine access veririk:
```
# k config set-context mzsamadov-context --cluster=kubernetes --user=mzsamadov@xalqbank.az
```
Sonra ise hemin contexte switch edirik:
```
# kubectl config use-context mzsamadov-context
Switched to context "mzsamadov-context".
root@k8s-master01:/kubernetes/kubernetes-course/authentication# kubectl config get-contexts
CURRENT   NAME                          CLUSTER      AUTHINFO                NAMESPACE
          kubernetes-admin@kubernetes   kubernetes   kubernetes-admin        default
*         mzsamadov-context             kubernetes   mzsamadov@xalqbank.az
```
Developere lazim olan sertifikati verdik ve o kubernetes klusterine qosula bilir lakin hec bir role-u olmadigina gore klusterde hecne ede bilmir:
```
# k get pod
Error from server (Forbidden): pods is forbidden: User "mzsamadov@xalqbank.az" cannot list resource "pods" in API group "" in the namespace "default"
```

***Davami Roled-base-access-control -dadir
