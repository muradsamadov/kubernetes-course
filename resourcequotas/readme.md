# ResourceQuota

Namespace yaradirsan ve teyin edirsenki bu ns daxilinde yalniz 3 configmap yaratmaq olsun, 10 servis yaratmaq olsun ve s. :
# k apply -f resourcequota.yml  --  resourcequota.yml faylinda etrafli hersey qeyd olunub.
# k apply -f helloworld-with-quotas.yml  --  helloworld-with-quotas.yml  faylinda da resourcequota.yml faylina uygun pod yaratmaq olar.