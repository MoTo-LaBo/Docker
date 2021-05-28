# Docker
Dcker Guideline
### Docker とは？
- 開発環境・実行環境をチームで揃える為のアプリ
- test 環境・本番環境も同じでなければ、挙動も変わってしまう
> ※ 実際に同じ開発環境を整えるのはすごく大変なこと。pc・OS皆違う。そこを簡単にしてくれるのが Docker
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
