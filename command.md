Docker command
### 1. docker hub に login
    docker login
1. username 入力
2. password 入力
3. Login Succeeded の表示で完了
### 2. docker pull で image を pull してくる
    docker pull
### 3. docker images で確認
    docker images
### 4. Container 作成 bash で操作 ※ container に入るためのコマンド
    docker run -it <image> bash
- docker container の中に (os)例)ubuntu (shell)bash の環境を構築
### 5. exit command を使用して Host に戻る
    exit
### 6. exit した場合の docker container 起動
     docker restart <name 又は ID>
### 5-1. detach : を使用して Host に戻る
- detach : ctrl + p+q 同時押し
### 6-1. detach を使用した時は、 attach で再度入る
    docker attach <container>
> ※ 基本は exit を使用する
### 7. container  (os)作業に戻る
    docker exec -it <container> bash
### コンテナを表示(アクティブなモノだけ) ※ process status の略
    docker ps
### 全ての docker container を表示 ※ 止まっているモノも含め
    docker ps -a
### docker image の一覧を表示
    docker images
### 8. Container から docker image へ変換
    docker commit <container> <new image>
## new docker image を docker hub へ push する
> ※ image と repository 名は一致していないといけない
### なんで image 名を repository 名に合わせるのか？
- docker は１つの image に対して１つの repository が対応
- docker は image を push する時に image の名前をみて push 先を決める
### image 名変更
    docker tag <source><target>
- 例）docker tag ubuntu:updated < user name>/my-first-repo
### docker hub に push する
    docker push <image>
### docker image を pull する
#### docker image を削除する
    docker rmi <image>
#### docker image を pull する
    docker pull <image>
#### 作業をするための環境(Container を立てる)
    docker run -it <image> bash
