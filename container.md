# container
- container をもっと理解する
- pc を使いこなす = Docker を使いこなす
- 今の tec 業界は、必須条件(環境構築)
## run について
- run = create + start
### create
    docker cerate
- create は container を作成するだけ
- cerated の状態
### start
- container を起動させて default のコマンドを実行させている
#### default のコマンドを見たい時は $ docker start -a
    docker start -a
## command を上書きする
    docker run <image><command>
- $ docker run ubuntu /bin/bash
   - default のcommand  は/bin/bash
   - default はそもそも bash
   - `-it`を指定しないとbash を開いた状態をkeep できない
## -it について
- $ docker run -`it` ubuntu bash
- -i : インプット可能 (入力チャネル)
   - stdin/stdout/stderr (というチャネルがある)
   - host から container につなぐ役割 (stdin)
- -t : 表示が綺麗になる
- ２つの option を重ねて -it となっている
