# Data Science Dockerfile
- data science に特化した Dockerfile 作成
- 実行環境(container)
  - base は ubuntu
  - ANACONDA
  - Jupyter lab
  - code は mount して hostの内容を見れるようにしておく -u option を使用
- Host
  - Dockerfile
  - code
### 1. build context 作成
    mkdir dsenv_biuld
- dsenv_biuld の directory 作成
  - data science environment creation
  - (データサイエンスの環境構築)
### 2. directory の中で Dockerfile 作成
    code . Dockerfile
- vs code で Dockerfile 編集
### 3. Dockerfile に記述
    FROM ubuntu:latest
    RUN apt-get update && apt-get install -y \
            sudo \
            wget \
            vim
    WORKDIR /opt
    RUN wget https://repo.continuum.io/archive/Anaconda3-2021.05-Linux-x86_64.sh
- package
  - **sudo**
  - ubuntu 上で root 以外の user が root 権限で実行できる
  - **wget**
  - HTTP を使用して data をとってくる
  - **vim**
  - エディター
- HTTP から download してきた data （.sh file）を実行することによって ANACONDA を install する事ができる
- .sh の file を root 直下に置いてしまうと、他の user が access しずらくなってしまう
  - /opt の root の下の opt の下に .sh file を download する
> ※ 共有 folder / file / data は、root (/) /opt の中に download file を入れる事は業務でもよく使用する！！
### wget を使用した理由
- host に ANACONDA を install して COPY (INSTRACTION) を使用してもいいが…後々の手間がかかる
- version を変更したい場合は Dockerfile を修正するだけで済む
  - https://repo.continuum.io/archive/以下にある`Anaconda3-2021.05-Linux-x86_64.sh` を自分の install したい ver に修正する
  - そして、 docker build → container
  - 上記だけで、ver の変更ができる
### 4. ANACONDA の installer を使用して install する
1. container に入って ANACONDA を install してみる
2. やり方がわかったら、改めてやり方を Dockerfile に記述していく
- Dockerfile を書いていく時は、最初からどういう事を記述していけばいいか分からない事が多い
- container に入って実際にコマンドを打ってやってみる
- 一般的には上記のようなやり方
### 4-1. PATH 「パスを通す」とは？
- 環境変数の $ PATH に、パスを追加することでPCが program をそのパスから探して来てくれるようになる
#### 今どこに PATH が通っているか確認
    echo $PATH
#### $PATH でパスを追加
    export PATH =/path/to/something:$PATH

### 4-2. インタラクティブなやりとりの回避
- Dockerfile を作成する上で考えないといけないこと
- install 途中で出てくる yes on no や file の download 先など...
- 色々とpcからinstall 中も聞かれる事がある
### 「 **.sh file (shell file)にはインタラクティブなやりとりを回避できる機能がついている** 」
#### .sh の機能・option の一覧表示
    sh -x Anaconda3-2021.05-Linux-x86_64.sh
- $ sh -x <実行file.sh>
- .sh がどのように使っていいか分からない時は確認
#### 今回は下記のような option が必要になってくる
- -b　　　　　run install in batch mode (without manual intervention)
  - バッチモードでインストールを実行する（`手動での操作は不要`）
- -p PREFIX　install prefix, defaults to /root/anaconda3, must not contain spaces.
  - インストールプレフィックス、`デフォルトは /root/anaconda3` スペースを含んではいけません。
### 4-3. flow
#### 1. $ docker run -it <image> bash
    docker run -it <image> bash
#### 2. $ sh Anaconda3-2021.05-Linux-x86_64.sh install する
    sh Anaconda3-2021.05-Linux-x86_64.sh
#### 3. $ export PATH=/opt/Anaconda3/bin パスを通す
    export PATH=/opt/Anaconda3/bin
- 上記で python のコマンドが使用できるようになる
#### 4. Dockerfile 記述の為に探る
    sh -x Anaconda3-2021.05-Linux-x86_64.
- どういう option が実行できるのか？
- -b -p を使うことによって、インタラクティブなやりとりを避ける事ができる
#### 5.
    sh /opt/Anaconda3-2021.05-Linux-x86_64.sh -b -p /opt/Anaconda3
### 5. Dockerfile 記述
    FROM ubuntu:latest
    RUN apt-get update && apt-get install -y \
        sudo \
        wget \
        vim
    WORKDIR /opt
    RUN wget https://repo.continuum.io/archive/Anaconda3-2021.05-Linux-x86_64.sh && \
    sh Anaconda3-2021.05-Linux-x86_64.sh -b -p /opt/anaconda3 && \
    rm -f Anaconda3-2021.05-Linux-x86_64.sh

    ENV PATH /opt/anaconda3/bin:$PATH

    RUN pip install --upgrade pip
    WORKDIR /
    CMD ["jupyter","lab","--ip=0.0.0.0","--allow-root","--LabApp.token=''"]
- -d
  - batch mode : バッチモードでインストールを実行する（`手動での操作は不要`）
- -p
  - install 先の PATH 指定 /optの下に(共有の為によく使用される)
- rm -f
  - installer を削除。ANACONDA3を install できればO K。 docker image 容量を削減の為にここで自動化して削除してしまう
  - -f : warning が出て確認で止まらないように、強制的に削除する
- ENV PATH <新しいPATH>:$PATH(今設定されているPATHに)
- package を管理する Tool pip(ピップ)
  - python の package 管理 tool
  - upgrade する
- WORKDIR /(root)に戻る。root から開始できる
