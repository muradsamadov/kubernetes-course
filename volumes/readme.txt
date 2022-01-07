StorageClass

deployment.yml faylinda storageclassin islemesi ucun lazim olan konfigler var, sadece oaraq deyisiklik olunacaq yerler nf-server ve nfs-path-dir, asagidaki kimi:
          env:
            - name: PROVISIONER_NAME
              value: k8s-sigs.io/nfs-subdir-external-provisioner
            - name: NFS_SERVER
              value: 192.168.52.250  # nfs-server ip
            - name: NFS_PATH
              value: /srv/nfs/kubedata/  # nfs-path
      volumes:
        - name: nfs-client-root
          nfs:
            server: 192.168.52.250  # nfs-server ip
            path: /srv/nfs/kubedata/  # nfs-path

rbac.yml fayli nece var elede qalsin ve hec bir deyisiklik olmur, storageclassin islemesi ucun lazimdir. Default olaraq gelir.
Daha sonra class.yml fayli yaradilir burada storage class yaranir
PVC ve avtomatik Pv-nin yaranmasida bu fayldadir test-claim-2.yml

# k apply -f .  --  butun yml fayllarini ise saliriq. deployment-2.yml faylinda nginx podu yaradiriq ve mount edirik nfs-servere.

Asagidakindanda gorunduyu kimi avtomatik olaraq PV yarandi ve Bound oldu:
[root@k8s-master01 dir]# k get pv,pvc
NAME                                                        CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                STORAGECLASS          REASON   AGE
persistentvolume/pvc-1c9e16f4-eb33-49ce-8be0-d0d560165508   1Mi        RWX            Delete           Bound    default/test-claim   managed-nfs-storage            29s

NAME                               STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS          AGE
persistentvolumeclaim/test-claim   Bound    pvc-1c9e16f4-eb33-49ce-8be0-d0d560165508   1Mi        RWX            managed-nfs-storage   48s

ls ederek nfs-pathe baxa bilerik.

[root@k8s-master01 dir]# ls /srv/nfs/kubedata/
default-test-claim-pvc-1c9e16f4-eb33-49ce-8be0-d0d560165508

---

Ikinci layihenide eyni qaydada yaradib hemin nfs-servere mount etmek olar. Bu halda qeyd olunan deployment-2.yml ve test-claim-3.yml fayli yaradilib orada baxmaq olar.