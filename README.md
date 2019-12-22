## Description
local環境でphoenixのinit createを行うと、elixirのversionやphoenixのversionを気にする必要があるため、毎回同じversion(最新 -> 必要であればversionをDockerfileを編集してfix)でprojectを立ち上げるために、docker環境内にelixirとphoenixをinstallして、projectの生成を行う

## Usage(command setup)
.gitを削除
> $ rm -rf .git

buildしてcontainerを作成
> $ cd phoenix

タグ付けに使用したい好きな名称でOK
tag_name=phoenix_server:dev 
> $ docker build -t tag_name .

containerを起動(--nameは起動するcontainerに認識子を与える。好きな名称でOK)
> $ docker run -dt --name elixir_train -p 8080:8080 elixir_test:dev

containerにlogin
> $ docker exec -it elixir_train ash

phoenixのprojectを生成(その他オプションは自由に付けてください)
> $ mix phx.new backend

生成したprojectをlocal環境にcopy
> $ sudo docker cp elixir_train:/app/backend .

## postgresの設定とdatabaseの作成
localにcopyしたphoenixの./project-name/config/dev.exsを編集
```
config :backend, Backend.Repo,
  adapter: Ecto.Adapters.Postgres,
  username: "root", # docker-compose.ymlに定義したPOSTGRES_USER
  password: "root_password", # docker-compose.ymlに定義したPOSTGRES_PASSWORD
  database: "backend_dev", # ecto.createコマンドを使わないので変更しても良いし、しなくても良い
  hostname: "db", # docker-compose.ymlに定義したservice名
  pool_size: 10
```

docker-composeを用いて各containerを立ち上げ
> docker-compose up --build

postgres containerにlogin
> $ docker ps

このcontainer idをcopy
> __9f676931727a__        postgres:12.0          "docker-entrypoint.s…"   ...

$ docker exec -it container-id /bin/bash
```
psql
root=#
```

./config/dev.exsに設定したdatabase名でdatabaseを作成
```
root=# create database backend_dev;
CREATE DATABASE
```

## default port information
phoenix -> localhost:4000
postgres -> localhost:5432
