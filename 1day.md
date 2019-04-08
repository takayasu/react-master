# React Master 　1Day

## Node.js について

### Nodejsとは
 [Node.jsとは（公式）](https://nodejs.org/ja/about/)

* サーバサイドJavascript？
* 非同期処理

モダンなJavascript開発環境のベースになる。
* React
* Vue.js
* Angular

## npm について
### npm とは
 [About NPM](https://docs.npmjs.com/about-npm/)

Nodeにおけるパッケージ管理誌システム。（Pythonのpipと同様）
* コマンドラインと設定ファイルの組み合わせ
* ツールなどのグローバル、プロジェクト固有（ランタイム含む）プロジェクト固有（開発のみ）

``` bash
npm install -g create-react-app
npm install --save redux
npm install --save-dev chai
```
### パッケージの設定ファイル



#### パッケージ管理システム（ライブラリ管理）
パッケージ管理システムにおいて、重要なことはバージョンが維持され、ライブラリを構成管理に格納せずに同じ環境が準備すできること。それがバージョンを固定するという意味となります。

## 開発環境の整備
Nodeはバージョンの進みが早いことと下位互換の少なさからバージョンを制御することは重要となる。そのため、複数バージョンを切り替えられるようにすることが望ましい。

### nodist
Node.jsを複数バージョン利用できる環境の一つ。Windowsでもちゃんと動いているので私はこれを使っています。但し、Windowsのみ。

 [nodist](https://github.com/nullivex/nodist)

``` bash
nodist dist
nodist add 11.0.2
nodist global 11.0.2
```


他にも以下のような環境がある。
* nvm
* nodebrew
* nodenv

## 宿題1
以下の指示にしたがい、手元のPCにNode開発環境を構築してください。
1. node環境を一つ(nodistなど)選択して、インストールをしてみましょう。その後で、Node.js 11.12.0 をインストールしてください。
1. この演習用にディレクトリを作成し、そのフォルダに移動してそのフォルダでのNode.jsのバージョンをインストールするバージョンにします。
1. npm init コマンドを実行してpackage.jsonを生成してください


