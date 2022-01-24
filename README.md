# laravel-docker

このリポジトリはLaravelをLocalでホスティングするためのDocker環境です

- nginx
- php7.4
- mysql5.7

<pre>
├── README.md
├── docker
│   ├── db
│   │   ├── data
│   │   ├── my.cnf
│   │   └── sql
│   ├── nginx
│   │   └── default.conf
│   └── php
│       ├── Dockerfile
│       └── php.ini
├── docker-compose.yml
├── server #このディレクトリがホスティングされる
└── server2 #このディレクトリがホスティングされる

</pre>

## 起動前準備
### Laravelプロジェクトを追加する
`./server/` に Laravelプロジェクト作成する、または、シンボリックリンクを作成する
```
# 作業ディレクトリはCloneしてきたプロジェクトのルートを想定
# シンボリックリンクを作成知る際には元のserverディレクトリは削除してください
ln -s [リンク先のProjectのpath] server
```

### .envのDB設定
```
DB_CONNECTION=mysql
DB_HOST=db
DB_PORT=3306
DB_DATABASE=test_db
DB_USERNAME=root
DB_PASSWORD=hogehoge
```   
## Docker起動
`docker-compose up` これで少し待つとホスティングが開始されます。
ブラウザにて http://localhost:8081 にアクセスするとLaravelへアクセスできます。

### migrateを行う
ルートディレクトリにて `docker-compose exec php bash` を実行することで
phpコンテナへログインすることができる。
ここで通常通り `php artisan migrate`

### Mysqlコンテナにログインする
`docker-compose exec db bash` の後に `mysql -uroot -phogehoge`もしくは
通常通り `mysql -uroot -phogehoge` を実行

### 複数のプロジェクトを同時にホスティングできるように修正
http://localhost:8082 にアクセスすると server2内のlaravelプロジェクトをホスティングできる


### ホスティング一覧
- server
    - test_db
    - http://localhost:8081
- server2
    - test_db2
    - http://localhost:8082