# Jupyter lab
-  browser 上で動く IP [y]: python の ide
-  browser 上で code を書いて code 結果がすぐに表示される
-  Jupyter notebook / Jupyter lab は1 user のみ
-  Jupyter hub は複数人で使用できる
## Why Docker ?
- Dockerfile → build → container さえしっかり作っていれば、環境構築で手間・時間を取ることはない
- 新たな開発に携わる時でも、Docker image → build → container で container さえ作ってしまえば、同じ環境下で作業が開始できる
  - Host の環境下に縛られく事もなく、皆どこでも同じ環境構築をできる
- Host と container は全く別のモノなので、Host 側の他のソフトウェアと confilct を起こしたり、環境構築で出る error なども全くなくなる
- docker image を build してしまえば、全員が同じ環境で作業できる
- container 自体はすぐに破棄できるから、アインストールなどの手間がない
- Docker を使わない方が逆に手間暇がかかり、本質の作業時間がなくなってしまう。そもそも環境構築するのが本質ではない。そういう作業は自動化してしまうのが一番で、自分のやるべき事に時間を割くべきだ。その為に Docker は最強の Tool と言える
### 「 環境構築は、あくまで本質ではなくその先の作業が大切。環境構築をストレスと時間をかけずに、簡単に早くできるように Docker を使用する 」

