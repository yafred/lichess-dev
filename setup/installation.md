## Installation

* MongoDB, redis and nginx installation is straight-forward (use your search engine)
* lila and lila-ws must cloned from https://github.com/lichess-org

### MongoDB

```sh
sudo mongod --dbpath ~/data/mongodb
```

### Redis

```sh
sudo service redis-server start
```

### nginx

Create /etc/nginx/sites-enabled/lila.conf with following content

```
map $http_upgrade $connection_upgrade {
    default upgrade;
    ''      close;
}



server {
   listen 9000;

    location /websocket/ {
        internal;

        proxy_pass http://localhost:9664/;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection $connection_upgrade;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

    location / {
        set $websocket 1;
        if ($http_connection !~* "upgrade") {
            set $websocket 0;
        }
        if ($http_upgrade !~* "websocket") {
            set $websocket 0;
        }
        if ($websocket) {
            rewrite ^ /websocket$uri last;
        }

        proxy_pass http://localhost:9663;
        proxy_http_version 1.1;
        proxy_set_header Host $http_host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        # proxy_set_header X-Lichess-KidMode 1;

        error_page 502 /502/lila.html;
    }

}
```
Then

```sh
sudo systemctl restart nginx
```

### lila-ws

```sh
git clone https://github.com/lichess-org/lila-ws.git
cd lila-ws
sbt run -Dcsrf.origin=http://localhost:9000
```

### lila

```sh
git clone --recursive https://github.com/lichess-org/lila.git
cd lila
```

You need to update conf/application.conf

```
# override additional values from base.conf here
net {
  domain = "localhost:9000"
  socket.domains = ["localhost:9000"]
}
```
Then
```sh
mongosh lichess < bin/mongodb/indexes.js # creates database indexes
ui/build # builds client. -h for help, and -w to automatically compile new frontend changes and see them when refreshing the browser.
./lila.sh # starts the SBT console
```
Once the console has booted, you will see a `lila>` prompt. Type `run` and sit back. 
Then open http://127.0.0.1:9000 in your browser (or http://localhost:9000 if that doesn't work).

> [Read more about the SBT console commands](https://www.playframework.com/documentation/2.8.x/PlayConsole).

