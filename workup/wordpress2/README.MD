# README

## wordpress

docker community provides a wordpress image maintained by docker

docker hub page : https://hub.docker.com/_/wordpress

git repo: https://github.com/docker-library/wordpress/

This is the Git repo of the Docker "Official Image" for wordpress (not to be confused with any official wordpress image provided by wordpress upstream)

the docker init file is modified to allow environment injection

https://github.com/docker-library/wordpress/blob/master/wp-config-docker.php

also possible to inject config using WORDPRESS_CONFIG_EXTRA:
https://github.com/docker-library/wordpress/pull/142  Here's an excerpt from my multisite docker-compose.yaml with
``` 
WORDPRESS_CONFIG_EXTRA: |
        /* Multisite */
        define('WP_ALLOW_MULTISITE', true );
        define('MULTISITE', true);
        define('SUBDOMAIN_INSTALL', false);
        define('DOMAIN_CURRENT_SITE', 'localhost'); // TODO: change to actual domain when deploying
        define('PATH_CURRENT_SITE', '/');
        define('SITE_ID_CURRENT_SITE', 1);
        define('BLOG_ID_CURRENT_SITE', 1);
```

## bitnami wordpress

bitnami has discontinued free images
https://www.blueshoe.io/blog/bitnami-and-alternatives/

bitnami example https://www.docker.com/blog/how-to-dockerize-wordpress/

https://github.com/bitnami/containers/blob/main/bitnami/wordpress-nginx/6/debian-12/rootfs/opt/bitnami/scripts/libwordpress.sh

## nginx session storage

https://bitbucket.org/nginx-goodies/nginx-sticky-module-ng/src/master/

https://hub.docker.com/r/exoplatform/nginx

https://serverfault.com/questions/832790/sticky-sessions-with-nginx-proxy

exo nginx with sticky
https://github.com/exo-docker/exo-nginx


https://github.com/nezhar/wordpress-docker-compose/blob/master/docker-compose.yml


https://blog.alphabravo.io/leveling-up-from-docker-compose-to-kubernetes-and-helm-with-wordpress/


https://nginx.org/en/docs/http/load_balancing.html