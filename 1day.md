# 基本概念と環境

## Node.js について

### Nodejsとは
 [Node.jsとは（公式）](https://nodejs.org/ja/about/)

* Javascript実行環境（ブラウザ外）
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

package.json というファイルにて管理されます。
初期ファイルは以下のような内容です。(npm init で作成できます)
``` json
{
  "name": "sample1",
  "version": "1.0.0",
  "description": "Sample1",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}
```

ここに上記コマンドでreactとchai ライブラリを入れると以下のようになります。

``` bash
npm install --save react
npm install --save-dev chai
```
によって、以下のように設定が追加されます。
``` json
  "dependencies": {
    "react": "^16.8.6"
  },
  "devDependencies": {
    "chai": "^4.2.0"
  }
```

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

# Single Page Application (SPA)

## レンダリング方法
### サーバサイドでHTMLをJavaなどでレンダリングする

### サーバサイドでHTMLをJavascriptでレンダリングする（SSR）

### DOMを操作してJavascriptでレンダリングする

## 宿題2
SPAが脚光を浴びている理由を3つ列挙してください。
また、SPAにすることによって難しくなったことを2つ上げてください。



# 画面コンポーネント
画面を分割して、再利用可能な部品の組み合わせで構成する考え方です。ネイティブアプリでは普通に不買われており、部品自体が販売されて、利用されています。

## 議論ポイント
* [最近のフロントエンドのコンポーネント設計に立ち向かう](https://qiita.com/seya/items/8814e905693f00cdade2)

* [再利用可能なコンポーネントはアンチパターン](http://jinjor-labo.hatenablog.com/entry/2016/08/03/031107)


## Web Components
画面コンポーネントをWebで適用とした規約が[WebComponents](https://developer.mozilla.org/ja/docs/Web/Web_Components)です。これは

* カスタム要素
* Shadow DOM
* HTMLモジュール（インポート含む）
* HTMLテンプレート
* CSS Change

の要素で構成されています。W3Cにて検討をすすめており、[文書](https://github.com/w3c/webcomponents)化されています。

この仕様に基づいてブラウザに[実装](https://caniuse.com/#search=components)されてきています。


## 画面コンポーネントの例と利用



## 画面の構造設計
* 画面のデータ構造と見栄えについて
* 画面共通と実装
* データ構造と再利用（状態の差異）
* 構成要素と粒度



### Atoms 設計

### Page/Container/Component

## 宿題3
今までのプロジェクト（設計が終わっていれば進行形でもよい）で、画面設計書をみて、画面分割をして構造的に設計するとどうなるかを検討して、画面設計書を作成してください。（一つだけでよいです）


# コンポーネント間でデータを渡す

## ステートの考え方
コンポーネントが動作のために盛っている状態をステートと呼びます。例えば、そのコンポーネントからダイアログを表示するような場合そのダイアログの表示・非表示がステートとにあたります。

このステートはコンポーネントで閉じている場合もあれば、コンポーネント間で共有されるものもあります。前はコンポーネントで考えればよいですが。そうでない場合はここでのテーマであるコンポーネント間でのデータを渡すこと考える必要があります。

## コンポーネントのデータ受け渡し
コンポーネントはデータが変更されるタイミングで、コンポーネントを必要としている親コンポーネントからデータを子コンポーネントに受け渡します。これはコンポーネントがどこで使われているかわからないことによる制約です。

逆に親子関係にないコンポーネント間でデータを受け渡しする必要がある場合、双方に共通の祖先までさかのぼり、バケツリレーをする必要があります。

## バケツリレーをやめる
このバケツリレーは単純なものでは成立しますが、コンポーネントが再利用できるレベルまでの細かいものでは実現できないことがわかります。

そのため、各コンポーネントが共通で参照できるStoreを作ることを考えます。この共通のStoreは更新と参照が入り乱れ、制御がしにくいことが想定されます。そのことから以下のFluxと呼ばれるアーキテクチャが考案されました。

## Flux
FliuxはStoreとStoreが操作されるイベントを明示化し。その更新・参照フローを直線的に実施するものです。
Reactを大規模システムに適用するためにFacebookで提唱されたものです。
Reactだけではなく、

* [Flux](https://github.com/facebook/flux)とは

全体像は以下の通りです。
![Flux](https://github.com/facebook/flux/blob/master/docs/img/flux-diagram-white-background.png?raw=true)


注目すべきはReactViewsから一方向で処理されているところです。

### Action

### Action Creators

### Dispatcher

### Store

### mapping(Change Event + Store Query)

# モダンJSの開発
## トランスパイル

## 単一モジュール化

## Web化
node.jsをベースに構築したソースコードをWebで動作させる必要がある。


## ビルド
ビルドが面倒。

* graunt
* gulp
* webpack




# 非同期処理


## Promise


# ブラウザとJavascript



# ES6

## 関数の書き方
JSでは以下の書き方で関数を作れるが
```javascript
function sample (arg1){
  return arg1
}
```
以下の書き方がES6っぽい。（function componentで利用するのでこちらを利用するほうがよい）
```javascript
const  sample =  (arg1) => return arg1
```
仮引数の括弧は省略できる。

## dictの取り扱い
``` es6
{style} = props
```

## モジュールのインポート、エクスポート


