#create folder mounted
```bash
mkdir -p ~/docker/mysql8-data
mkdir -p ~/docker/mysql8-conf
sudo chown -R 999:999 ~/docker/mysql8-data
sudo chmod -R 777 ~/docker/mysql8-data
```

#file my.inf
```bash
[mysqld]
lower_case_table_names=1
sql_mode=STRICT_TRANS_TABLES,NO_ENGINE_SUBSTITUTION
datadir=/var/lib/mysql

socket=/var/run/mysqld/mysqld.sock
pid-file=/var/run/mysqld/mysqld.pid

mysqlx_socket=/var/run/mysqld/mysqlx.sock

bind-address=0.0.0.0

[client]
socket=/var/run/mysqld/mysqld.sock
```

```bash
docker stop mysql8 2>/dev/null
docker rm mysql8 2>/dev/null
```

```bash
sudo rm -f \
  "$HOME/docker/mysql8-data/mysql.sock" \
  "$HOME/docker/mysql8-data/mysql.sock.lock" \
  "$HOME/docker/mysql8-data/mysqlx.sock" \
  "$HOME/docker/mysql8-data/mysqlx.sock.lock"
```
```bash
docker run --name mysql8 \
  --restart unless-stopped \
  -e MYSQL_ROOT_PASSWORD=123456 \
  -e MYSQL_DATABASE=admin \
  -p 3306:3306 \
  -v "$HOME/docker/mysql8-data:/var/lib/mysql" \
  -v "$HOME/docker/mysql8-conf/my.cnf:/etc/mysql/conf.d/my.cnf:ro" \
  -d mysql:8.0-debian
```
