# ENV
podenv.yml faylina esasen 2 dene env set olunub. Podu apply ederken 'env' komandasi ile set olunan env-lara baxmaq olar:
```
# k exec -it envpod -- env
PATH=/usr/local/apache2/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
HOSTNAME=envpod
TERM=xterm
USER=Murad
database=localhost
BACKEND_SERVICE_PORT=5001
BACKEND_PORT_5001_TCP=tcp://10.109.209.104:5001
BACKEND_PORT_5001_TCP_ADDR=10.109.209.104
```