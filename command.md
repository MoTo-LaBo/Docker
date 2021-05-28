# Docker flow
### 1. docker hub に login
    docker login
- username 入力
- password 入力
- Login Succeeded の表示で完了
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
### 6. Container 作成
    docker run -it ubuntu bash
- docker container の中に (os)ubuntu (shell)bash の環境を構築
- 普通ならもっと色々と付け加えていく
### 7. touch < test >
    touch < test >
### 8. exit command を使用して Host に戻った
    exit
### 9. exit した docker container 起動
     docker restart <name 又は ID>
- コマンド起動
### 10. container  (os)作業に戻る
    docker exec -it <container> bash
### 11. detach : を使用して Host に戻る
- detach : ctrl + p+q 同時押し
### 12. detach を使用した時は、 attach で再度入る
    docker attach <container>
## detach と exit の違い
### exit
- container を出る時に container を動かしている process を切って出る
### detach
- process を残したまま cantainer から出る
> ※ 基本は exit を使用して container から出る
