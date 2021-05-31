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
