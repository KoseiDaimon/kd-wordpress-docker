# 大門光星の WordPress in Docker の開発環境

## 必須環境
- Dockerが使える状態

## 機能
- WordPress用のDockerコンテナ（WordPress + MySQL）を作成できます
- 複数のサイトを同時に起動できます
- 各サイトに対応した好きなコンテナ名、IPアドレスを設定できます
- 「wp-content」ディレクトリと、本ディレクトリは自動同期されます
-

## 使い方
### １．プロジェクト名の指定
- ダウンロードしたフォルダを解凍し、開きます。
- 解凍したフォルダの名前をプロジェクト名（ローマ字、ハイフンのみ使用可）に変更します。

### ２．Dockerの起動
1. 下記コマンドで、現在使用されているIPアドレスを確認します。
```
docker ps -a
```

2. 「.env」にて [ IP_ADDRESS ] を使われていないIPアドレスに変更します。
```
【例】
# IP Address for Docker Containers
IP_ADDRESS=127.0.0.1
```

3. 「.env」にて [ CONTAINER_NAME ] をわかりやすい名前に変更します。（半角英数字 + ハイフン）
```
【例】
# Docker Container Name
CONTAINER_NAME=test-container
```

4. MySQLのバージョンを指定したい場合は、「docker-compose.yml」にて、[ image: mysql:5.7 ]を変更します。
```
【例】
# IP Address for Docker Containers
IP_ADDRESS=127.0.0.1
```

5. WordPressのバージョンを指定したい場合は、「docker-compose.yml」にて、[ image: wordpress:latest ]を変更します。
  参考： https://hub.docker.com/_/wordpress/tags

1. 下記コマンドで、Dockerを起動します。
  ```
  docker-compose up -d
  ```
1. 下記コマンドで、「任意のテーマ名_wordpress」と「任意のテーマ名_db」の２つのコンテナが起動していることを確認します。
  ```
  docker ps -l
  ```

## Dockerの削除方法
- 下記コマンドで、Dockerを削除できます。
  ```
  docker-compose down -v
  ```