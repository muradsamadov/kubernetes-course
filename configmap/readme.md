ConfigMap

# k create configmap nginx-config --from-file=reverseproxy.conf  --  ederek nginx-config yaradiriq. 
# k apply -f nginx.yml -- helloworld-nginx adinda pod yaradilir ve daxilindede gorunduyu kimi nginx-config configmap elave olunur bu poda. Bu halda reverseproxy.conf konfig faylini containere elave edirik.

Asagidaki komanda ile configmapa daha detalli baxmaq olur:

[root@k8s-master01 configmap]# k describe cm nginx-config 
Name:         nginx-config
Namespace:    default
Labels:       <none>
Annotations:  <none>

Data
====
reverseproxy.conf:
----
server {
    listen       80;
    server_name  localhost;

    location / {
        proxy_bind 127.0.0.1;
        proxy_pass http://127.0.0.1:3000;
    }

    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
}


BinaryData
====

Events:  <none>