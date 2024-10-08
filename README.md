<p align="center">
  <h1 align="center">KD-WORDPRESS-DOCKER</h1>
</p>


## 必須環境
- Dockerが使える状態



## 機能
- WordPress用のDockerコンテナ（WordPress + MySQL + phpMyAdmin）を作成できます。
- ポート番号を分けることで、複数のサイトを同時に起動できます。
- 各サイトに対応した好きなコンテナ名を設定できます。
- 本ディレクトリに、`wp-content`と`upload.ini`が自動同期されます。



## 使い方
### １．プロジェクト名の指定
- ダウンロードしたフォルダを解凍し、開きます。
- 解凍したフォルダの名前をプロジェクト名（ローマ字、ハイフンのみ使用可）に変更します。

### ２．Dockerの起動
1. 下記コマンドで、現在使用されているポート番号を確認します。
  ```
  docker ps -a
  ```

2. `.env.example`を`.env`に変更します。

3. `.env`にて、`PORT_WORDPRESS`と`PORT_PHPMYADMIN`を使われていないポート番号にします。
  【例】
  ```
  # Port Numbers of Docker Container
  PORT_WORDPRESS=8001
  PORT_PHPMYADMIN=8002
  ```

4. `.env`にて、`CONTAINER_NAME`を使われていない、わかりやすい名前に変更します。（半角英数字 + ハイフン推奨）
  【例】
  ```
  # Docker Container Name
  CONTAINER_NAME=your-container-name
  ```

5. `.env`にて、各コンテナのバージョンを指定します。（任意）
  ※参考： https://hub.docker.com
  【例】
  ```
  # Image Tag of Docker Container
  # https://hub.docker.com/
  IMAGE_TAG_MYSQL=5.7
  IMAGE_TAG_WORDPRESS=latest
  IMAGE_TAG_PHPMYADMIN=latest
  ```

6. サブディレクトリにWordPress関連ファイルを設置する場合は下記を設定する。
例：/wordperss/ ディレクトリにいれる場合
  ```
  volumes:
    - ./html:/var/www/html
    - ./conf.d/upload.ini:/usr/local/etc/php/conf.d/upload.ini
  working_dir: /var/www/html/wordpress
  ```

7. 下記コマンドで、Dockerを起動します。
  ```
  docker-compose up -d
  ```
  ※起動に失敗した場合は、再度実行したりしてください。

8. 下記コマンドで、3つのコンテナが起動していることを確認します。
  ```
  docker ps -n 3
  ```

### ３．WordPressをインストール
1. WordPressにアクセス
  `.env`の`PORT_WORDPRESS`を確認して、Webブラウザでアクセス
  【例】
  ```
  localhost:8001
  ```

  ※サブディレクトリに構築した場合は、下記のようにアクセス
  【例】
  ```
  localhost:8001/wordpress
  ```

2. 言語を選択して、「次へ」をクリック

3. 必要情報を入力して、「WordPressをインストール」をクリック

## phpMyAdminへアクセス
`.env`の`PORT_PHPMYADMIN`を確認して、Webブラウザでアクセス
【例】
```
localhost:8002
```

## 作業の中断
作業中断時にDockerコンテナを削除したい場合は、下記コマンドを実行します。
```
docker compose down
```

## 作業の再開
```
docker compose up -d
```

## WordPressのアップロードファイルのサイズ上限を変更
1. `conf.d`ディレクトリ内の、`upload.ini`ファイルに、下記の内容を記載する。
【例】1GBまでアップロード可にする場合
```
upload_max_filesize = 1000M
post_max_size = 1000M
```

2. 下記コマンドを実行
```sh
docker-compose restart
```

## phpMyAdminのアップロードファイルのサイズ上限を変更
```
environment:
  MEMORY_LIMIT: 128M
  UPLOAD_LIMIT: 64M
```

## このDocker Compose関連のデータを全て削除
下記コマンドで、`docker-compose up -d`で作成されたコンテナ、イメージ、ボリューム、ネットワークを停止して削除できます。
※データが全て削除されるので注意してください。
  ```
  docker-compose down --volumes --remove-orphans
  ```

## WSLのディレクトリで動かす場合
権限の関係でDockerコンテナ内のファイルを編集できないことがあります。（permission denied）
下記のように変更します。
※これはwslのデフォルトユーザーをrootにする設定です。
  Dockerコンテナ内だけでなく、常に全てのファイルを編集できてしまうことに注意が必要です。
```conf:wsl.conf
[user]
default=root
```