# Docker
Dcker Guideline
### Docker とは？
- 開発環境・実行環境をチームで揃える為のアプリ
- test 環境・本番環境も同じでなければ、挙動も変わってしまう
> ※ 実際に同じ開発環境を整えるのはすごく大変なこと。PC・OSは皆違うので、そこを簡単にしてくれるのが Docker
- コンテナという技術を使って、コンテナの中で同じ環境を作る
### Docker file / image
- docker file は txt file , docker image の設計図
- docker image はコンテナを作成する元となるもの
- docker image からは、違うコンテナをそれぞれ作ることができる
### Docker Hub flow
1. docker imag が沢山保存されている
2. Host(my pc)に docker image を取ってきて、docker から docker image を使ってコンテナを作る
3. コンテナに入って作業をして、新しい docker image を作成
4. 新しい docker image をdocket hub に push する
### 「 Docker container を使用するから、とても最小限の code で済む 」
- 開発環境の構築がとても楽に効率的にできる
  - いちいち、local に CI に deploy にと３つに同じ環境を作ろうとする事が凄く大変
  - 労力と時間の無駄。Docker container １つでOK
  - チームに誰か入って来た時でも、GitHub に code は上がっているから Docker を使用していれば clone して、Docker copomse up をその人の環境でしてしまえば container は作成出来てすぐに、開発に入れる
- 運用面・メンテナンス・効率面・煩雑さをなくす etc...
- 無駄な作業を失くし、業務の効率をあげる上でも使用しないという選択肢は無い
## 『 Docker を使用することで、開発の全てがスムーズになる 』
- Docker だけでは何も意味を持たない
- 他のtool と組み合わせることによって、開発環境、test 環境、CICD 環境、deploy 環境が凄く簡単（container）に、効率的(自動化)に出来る
- フレームワーク、DB、CI tool、server どんなものであれ、基本的には Docker を使うという事では何も変わらない。全体のflow は一緒
