# 仮プロジェクト起動手順

以下、仮プロジェクトをDockerを使って、起動/開発が出来るようにする一連の流れです

## Requirements

- Git
- Docker
- Docker-compose

## Usage

- GitHubからクローンしてコンテナを立ち上げる

```cmd
$ git clone https://github.com/jink-D/nuxt_project.git
$ cd nuxt_project
$ docker compose up -d
```

- コンテナ内に入ってnpx nuxiを使用しnuxt/starterをインストール

```cmd
$ docker-compose exec app sh
```

```sh
/src # npx nuxi init .
```

- docker-composeを書き換えDockerを再起動

```diff
version: '3.8'

services:
  app:
    build: .
    volumes:
      - ./src:/src:cached
-       # - node_modules:/src/node_modules
+       - node_modules:/src/node_modules
      # ↑ 次回起動時コメントアウトを解除
    working_dir: "/src"
    ports:
      - "3000:3000"
    tty: true
    environment:
    - HOST=0.0.0.0
    - port=3000
    - CHOKIDAR_USEPOLLING=true

volumes:
  node_modules:
```

```sh
/src # exit
$ docker compose down 
$ docker compose up -d
```

- Nuxt 3のインストール

```sh
$ docker-compose exec app sh
/src # npm install
```

- Nuxt3 起動
```sh
$ docker compose exec app sh
/src # npm run dev
```

- Dockerの起動と終了
    - 起動
    ```cmd
    $ docker compose up -d
    ```
    - 終了
    ```cmd
    $ docker compose down
    ```

### docker endpoint for default not found のエラーが出た場合
以下のコマンドでmeta情報を削除してから再ビルドしてください
```sh
$ rm -rf C:\Users\<ユーザ名>.docker\contexts\meta\XXXXXX
```

### 参考サイト
- URL: <https://zenn.dev/szn/articles/nuxt-3-with-docker-compose>