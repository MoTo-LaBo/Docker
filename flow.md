# Docker flow
### 1. docker hub に login
    docker login
1. username 入力
2. password 入力
3. Login Succeeded の表示で完了
> ※ インターネットに繋がってないと login できない！！
### 2. docker pull で image を pull してくる
    docker pull
- Host に image を pull
### 3. docker images で確認
    docker images
### 例）下記が表示される
    REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
    hello-world   latest    d1165f221234   2 months ago   13.3kB
#### library は下記のurl から確認できる
> ※ https://hub.docker.com/u/library
### 4. コンテナを作成
    docker run < images >
### 5.  コンテナを表示(アクティブなモノだけ)
    docker ps
#### 全ての docker container を表示
    docker ps -a
### 6. Container 作成 bash で操作
    docker run -it ubuntu bash
- docker container の中に (os)ubuntu (shell)bash の環境を構築
- 普通ならもっと色々と付け加えていく
### 7. touch < test file > 作成
    touch < test >
### 8. exit command を使用して Host に戻る
    exit
### 9. exit した場合の docker container 起動
     docker restart <name 又は ID>
- コマンド起動
### 10. container  (os)作業に戻る
    docker exec -it <container> bash
### 8-1. detach : を使用して Host に戻る
- detach : ctrl + p+q 同時押し
### 9-1. detach を使用した時は、 attach で再度入る
    docker attach <container>
## detach と exit の違い
### exit
- container を出る時に container を動かしている process を切って出る
### detach
- process を残したまま cantainer から出る
> ※ 基本は exit を使用して container から出る
### Container から docker image へ変換
    docker commit <container> <new image>
## new docker image を docker hub へ push する
- その前に docker hub で 自分の repository を作成
- 新しい image に対して 1つの repository を作成
   - image / repository は１つずつだが tag 別に色々な種類の image を保存している
### image 名
- repository名 : tag 名 = image 名
   - tag 名 を指定しない場合は default で latest になる
- 例）
   - library/ubuntu : latest = ubuntu image 名
> ※ image と repository 名は一致していないといけない
### なんで image 名を repository 名に合わせるのか？
- docker は１つの image に対して１つの repository が対応。docker は image を push する時に image の名前をみて push 先を決める
### image 名変更
    docker tag <source><target>
- 例）docker tag ubuntu:updated < user name>/my-first-repo
### 他の場所へ push する場合
- 他の場所へpush する場合は < host name >:< port > を送信先のもに設定して push する
### image name 構成
`< host name >:< port >/< username >/< repository >:< tag >`
- default の設定
   - < hostname >:< port > → registry-1.docker.io
   - < username >  　　　　→ library
   - < tag > 　　　　　　 　 → latest
#### 正式な repository の name
`regirstry-1.docker.io/library/ubuntu:latest`
### docker hub に push する
    docker push <image>
### docker image を pull する
#### docker image を削除する
    docker rmi <image>
#### docker image を pull する
    docker pull <image>
#### 作業をするための環境(Container を立てる)
    docker run -it <image> bash

