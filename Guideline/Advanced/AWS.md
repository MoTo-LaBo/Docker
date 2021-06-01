# AWS と Docker
- Docker の環境を送る為の step
- Docker を使っていく上で解析サーバーやサーバーを立てるという事は必須になってくる
- Docker は環境を portable なものにして container を server に置いたり、色々な人に配ったりできる
  - cloud server を使ってそこに Docker を立てるという事は非常に相性が良い
### AWS に Docker 環境の送り方
1. Docker レジストリを使う
2. Dockerfile を送る
3. Docker image を tar(圧縮file) にして送る
- 大きく分けれ３つのやり方がある
## AWS の登録
- cloud service とは？
  - 色々な server を持っていてそこの一部を使わせてもらう
  - server を買う必要もないし、必要な分だけ server を借りれば安く済む
  - Amazon が server を管理しているので、セキュリティー面でも気にする必要がない
  - 使う時だけ、時間単位でハイスペックのコンピューターを使うことができる
## AWS Host : EC2 を立てて SSH でログイン
- Host(aws) / local (mac)
- $ chmod 400 < file >
- 4 → 所有者　0 → 所有グループ　0 → その他
  - パーミッションを変更する事で access 権限を強化
  - 4: read
  - 2: write
  - 1: execute
  - 0: no permission
  - 4 + 2 + 1 = 7 rwe (全てできる) とういう感じ
> ※ 鍵 file を自分が編集できたらログインできなくなるから、自分自身は read で他は権限なし
### file 権限強化
    chmod 400 mydocker.pem
### ssh でログイン
    ssh -i xx.pem username@hostname
- hostname
  - パブリック IPv4 DNS
  - **インスタンスの status を stop にして run にする毎に変わるので注意**  毎回ログイン毎に、パブリック IPv4 DNS を確認して ssh ログイン時に記述
- username
  - ubuntu / 今回は ubuntu を立てたので default で ubuntu になっている
## EC2 に docker を install する
### 1. $ sudo apt-get update
    sudo apt-get update
- root 権限として実行 → sudo
### 2. $ sudo apt-get install docker.io
    sudo apt-get install docker.io
- docker には２つ種類があって
- docker ce　→　docker.io を install すれば docker ce が install される
  - コミニケーション エディション
- docker ee
  - エンタープライズ エディション (企業が使うガチのヤツ)
### 3. docker ver 確認
    docker --version
- version が表示されればしっかりと docker が install された証拠
### 4. permission denied (ubuntu user は docker に対して権限がない)
- docker コマンドを使用すると permission denied の表示が出る
- sudo を付ければ docker コマンドは実行できる
  - 毎回 sudo をつけるのは効率が悪い
  - root 権限としての実行なので、あまり良くない
### 5. docker という user group を作ってそこに入れる
- docker group に所属している user は docker を自由に使えるようになる
### 「 **ubuntu server を使う時には必ずと言って良いほどやる事なので覚えておく！！** 」
#### ubuntu を docker group に入れる
    sudo gpasswd -a ubuntu docker
- Adding user ubuntu to group docker が表示される
#### 一度 ログアウトして、またログイン
    docker images
- そうすると、sudo を付けなくても docker コマンドが使用できるようになっている
## EC2 に　Docker image/Dockerfile を送る
1. Docker レジストリを使う
   - loacl から Docker image を Docker Hub に push する
   - AWS 側で pull する
   - 業務で一番多いやり方。Docker のレジストリを用意していれば一番やりやすいやり方
2. Dockerfile を送る
   - 送った先で build する → Docker image → container
   - このやり方だと build context が変わる可能性がある
   - local と全く同じ Docker image を作れるとは限らない
   - build context の内容も使用する container だと、build context の内容が変われば、 container の内容も必然的に変わる
   - スリムなやり方、Docker image はどうしても容量が大きく重たくなってしまう。Dockerfile は、txttfile なので容量も小さく軽く送る事ができる
3. Docker image を tar にして送る
   - Docker image. tar (圧縮file) それを送る
   - サイズを小さくして送りたい為
   - このやり方だと、別 server(docker hub)を介さずに EC2(AWS)に直接送る事ができる
### どういう時に tar にして送るか?
- data が重要な個人情報を持っているdataで、serverはかなり secure な server でなければならない。access 制限がかなり厳しくなっていて、他のインターネットや server に access できなくなる事が多い
- 医療 data ect...
- そういうケースではない時は、1. 2. を使用することが多い
## Dockrfile を tar にして送る
### 1. dockerfile 作成 → docker image を作成
    docker build .
### 2. docker image を tar file にする
    docker save 10b329d > myimage.tar
- 上記のコマンドで tar file が作成される
- $ docker save (image ID) > (file 名).tar
### 3. SFTP : Secure File Transfer Protocol
#### 1. SFTP を使って tar file を EC2(aws)へ upload する
    sftp -i mydocker.pem ubuntu@<hostname>
- SSH は shell を使うためのもの SFTP は file を transfer(転送) させるもの
#### 2. EC2 server へ送る
    put local/path [remote path]
- $ put temp_folder/myimage.tar home/ubuntu
#### 3. EC2 server から data をとってくる
    get remote/path [local/path]
- $ get something
#### 4. EC2 で tar file を docker image → container に変換
##### 1. SSHログイン
    ssh -i mydocker.pem ubuntu@hostname.amazonaws.com
##### 2. tar file を docker image に変換
    docker load < myimage.tar
##### 3. container を作成
    docker run -it myimage bash
