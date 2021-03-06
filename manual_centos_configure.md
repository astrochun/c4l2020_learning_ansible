## postgresql (on c4l20node1)

As the super-user

Edit `/var/lib/pgsql/12/data/postgresql.conf` (about line 59)

```ini
...
listen_address = '*'
...
```

Edit `/var/lib/pgsql/12/data/pg_hba.conf` and add

```ini
# allow web machines
host    all        all        10.0.15.12/32        md5
host    all        all        10.0.15.13/32        md5
```
Create db user and db as the `postgres` user

```bash
createuser -P -d -e c4l20_drupal_user
createdb -O c4l20_drupal_user c4l20_drupal_db
```

## solr (on c4l20node1)

```bash
sudo yum install lsof
```

## nginx (on c4l20node2 and c4lnode3)

add index.php and
uncomment the following on  PHP location block of the `/etc/nginx/conf.d/default.conf` nginx configuration file

```
server {
...
  location / {
      root   /usr/share/nginx/html;
      index  index.html index.htm index.php;
  }
...
...
  location ~* \.php$ {
    root  /usr/share/nginx/html;
    fastcgi_pass unix:/run/php-fpm/www.sock;
    include         fastcgi_params;
    fastcgi_param   SCRIPT_FILENAME    $document_root$fastcgi_script_name;
    fastcgi_param   SCRIPT_NAME        $fastcgi_script_name;
...
```
