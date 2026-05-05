#  Running MySQL with Docker

To set up the database for this project, I used a MySQL container on my VM instead of installing it manually.


## Step 1:  Pull the MySQL Image

First, I pulled the latest MySQL image:

```bash id="r5o9i8"
docker pull mysql:latest
```

## Step 2:  Run the MySQL Container

Then I started the container with the required environment variables and a volume for persistence:

```bash id="g4k2pl"
docker run -d \
  --name mysql-container \
  -e MYSQL_ROOT_PASSWORD=my-secret-pw \
  -e MYSQL_DATABASE=book_review_db \
  -e MYSQL_USER=pravin \
  -e MYSQL_PASSWORD=Demo12@Test23 \
  -p 3306:3306 \
  -v mysql_data:/var/lib/mysql \
  mysql:latest
```


## Step 3:  Check if Everything is Running

```bash id="9r6m2n"
docker ps
```

If everything is fine, the MySQL container should appear as running.


## Step 4:  Connect to MySQL

To access the database from inside the container:

```bash id="c3k7fp"
docker exec -it mysql-container mysql -u root -p
```

After entering the password, I checked the databases:

```sql id="x7n2ad"
SHOW DATABASES;
```

You should see:

```id="v8k2lm"
book_review_db
```

## Step 5:  (Optional) Allow Remote Access

If you need to connect to MySQL from outside the VM:

### Open the port:

```bash id="u6p3sj"
sudo ufw allow 3306/tcp
```

### Allow external connections inside MySQL:

```bash id="t9k4wd"
docker exec -it mysql-container bash
```

Edit the config file:

```bash id="p2d8qn"
nano /etc/mysql/mysql.conf.d/mysqld.cnf
```

Change:

```id="a1x7zd"
bind-address = 127.0.0.1
```

To:

```id="b5n9rt"
bind-address = 0.0.0.0
```

Then restart the container:

```bash id="h4v8yk"
docker restart mysql-container
```
