  - name: Deploy Wordpress
    shell: |
      kubectl create deployment wordpress --image wordpress:5.1.1-php7.3-apache
      kubectl expose deployment wordpress --type NodePort --port 80

      kubectl created deployment mysql  --image mysql:5.7 --env MYSQL_ROOT_PASSWORD=redhat     --env MYSQL_DATABASE=wpdb  --env MYSQL_USER=siva  --env MYSQL_PASSWORD=redhat

      kubectl expose deployment mysql --type ClusterIP --port 3306

      exit 0
    delegate_to: remote

  - name: Printing DB Cluster IP and port
    cmd: kubectl get svc mysql
    register: mysql_host

  - name: MySQL HOST:PORT
    debug:
      var: mysql_host.stdout

  - name: Priniting WordPress IP
    cmd: curl ifconfig.me
    register: ip

  - name: wordpress IP
    debug:
      var: ip.stdout

  - name: Printing Wordpress Port
    cmd: kubectl get svc wordpress
    register: wordpress_host

  - name: Wordpress port
    debug:
      var: wordpress_host.stdout
