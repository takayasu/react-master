# ブラウザとJavascript

 * [W3C HTML5.1 2nd](https://www.w3.org/TR/html51/)

## HTMLレンダリングエンジン
HTML、CSSのパースや画面描画はレンダリングエンジンと呼ばれる構成要素によって実現されています。

* [ブラウザの仕組み: 最新ウェブブラウザの内部構造](https://www.html5rocks.com/ja/tutorials/internals/howbrowserswork/)

* [Life of a Pixel 2018](https://docs.google.com/presentation/d/1boPxbgNrTU0ddsc144rcXayGA_WF53k96imRH8Mp34Y/edit#slide=id.g26f22ee049_1_94)

p9 stages をみてレンダリングパイプラインに目を通してください


### レンダリングエンジン
* [Gecko](https://developer.mozilla.org/en-US/docs/Mozilla/Gecko)
* [WebKit](https://webkit.org/)
* [Blink](https://www.chromium.org/blink)

#### DOM (Document Object Model)
HTMLパースして作られた構成要素をオブジェクト構造で表現したもの。
このDOMに基づいて画面を描画する。

* [ドキュメントオブジェクトモデル (DOM)](https://developer.mozilla.org/ja/docs/Web/API/Document_Object_Model)

DOMを操作するJavascriptのAPIも含む

## HTTP通信
ブラウザはURLに基づいて、適切な通信を実施してデータを取得します。

* [非公式らしいですが日本語のRFC](https://triple-underscore.github.io/RFC723X-ja.html)


### HTTP リクエスト
HTTP通信をおこなうためのリクエストの仕様。特にヘッダが重要。

* [RFC 7231](https://httpwg.org/specs/rfc7231.html#request.header.fields)


### HTTP レスポンス
基本はHTML。但し、ステータスコードとヘッダは重要。

#### ステータスコード
通常時は200（OK）

* [RFC7321](https://httpwg.org/specs/rfc7231.html#status.codes)

#### レスポンスヘッダ

* [RFC7321](https://httpwg.org/specs/rfc7231.html#response.header.fields)

|ヘッダ|説明|
|---|---|
|Content-Type|表示のフォーマットを表すヘッダ。|
|Expires|コンテンツの有効期間|
|E-Tag|コンテンツの状態を表すヘッダ。この値が一致している場合コンテンツは変更されていないことを表す。（キャッシュの有効性などで利用される）|

## Javascriptエンジン

* [V8](https://v8.dev/)
* JavascriptCore (Nitro) --WebKit組み込み
* [SpiderMonkey](https://developer.mozilla.org/en-US/docs/Mozilla/Projects/SpiderMonkey)

## APIの提供
ブラウザの機能を拡張するためのAPIが提供されています。

* [JavaScript API 群](https://developer.mozilla.org/ja/docs/Mozilla/Add-ons/WebExtensions/API)



## 表示のプラグイン
ブラウザで表示するものはHTMLだけではなく、PDFをはじめとする別のファイルフォーマット（映像なども含む）を表示することも意図されます。
この識別はContent-Typeによっておこなわれます。この値はMIMEタイプが指定されます。

### MIME(Multipurpose Internet Mail Extention)
もともとはメールで添付メールを処理するための規格。
データフォーマットを定義しているため、Content-Typeヘッダーの値として用いられます。
タイプ/サブタイプで表されます。

|表示|フォーマット種類|
|---|---|
|image/jpeg|JPEG画像|
|application/json|JSON|
|text/html|HTML|

* [MIME タイプの不完全な一覧
](https://developer.mozilla.org/ja/docs/Web/HTTP/Basics_of_HTTP/MIME_types/Complete_list_of_MIME_types)


## 宿題0
ブラウザの開発環境を用いて、以下の内容を試してください
* コンソールにHello JSWorld! と出力する
* 適当な場所（Googleなど）を表示して、画面の上部に適切なメッセージを追加して描画してください。
* あるキーワードをブラウザに保存して、一度ブラウザを閉じてからそのキーワードを取り出してください。

# 基本概念と環境
もともとはブラウザの言語だったJavascriptを別環境で動作させることを考え始めた。

Javascriptは言語として標準化され実装されるようになった。

* [ECMAscript](https://jsprimer.net/basic/ecmascript/)


## Node.js について

### Nodejsとは
 [Node.jsとは（公式）](https://nodejs.org/ja/about/)

* Javascript実行環境（ブラウザ外）
* シングルスレッド
* 非同期処理
* イベント駆動

モダンなJavascript開発環境のベースになる。
* React
* Vue.js
* Angular

## 非同期処理
非同期処理とは、タスク実行中でも他のタスクが実行出来る状態を表します。このことはIO待ちなどのCPUを効率よく利用出来ない局面において効果的です。

### 並行処理と並列処理
* [並行処理と並列処理](http://freak-da.hatenablog.com/entry/20100915/p1)

NodeJSでは、シングルスレッドであるため、並行処理になります。
待ちが発生する処理（ファイルIO、HTTP通信など）は具体的にはどのような処理になるのかをみてみます。

### コールバック

```javascript
var request = require('request');

request.get('http://www.google.co.jp', function(err, res, body) {
  if (err) {
    console.log(’Error’,err);
    return;
  }
  console.log(res);
});
console.log('通信中');
```
この例ではrequestモジュールを利用して、HTTP通信をしています。通信リクエストがされた後は処理は次に進むため、通信中という文言がコンソールに出力されます。

ここでの無名関数 *function(err, res, body)* がコールバックになっており、HTTP通信完了後に呼び出されます。ここでの関数は無名関数（わかりやすいようにfunctiondeで記載した）だけでなく、アロー関数での記述や関数を格納した変数を設定することも可能です。

### Promise
コールバック関数は一つであれば特に問題ではないが、コールバックの中から非同期処理を実施したり、エラー処理をしたりするなどネストされることが想定され可読性の低いソースコードになることが懸念されます。（実際に多かった）そのため、Promiseと呼ばれる仕組みを利用します。

```javascript
var requestPromise = require('request-promise');
var promise = requestPromise('http://www.google.com')

promise.then((success)  => {
      console.log(success)
    }).catch( (error) => {
      console.log('Error',error)
});
```
requestモジュールのPromise版であるrequest-promiseを用いてコールバック版をPromiseで書き換えたものです。

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
1. 上記で記載したHTTP通信をおこなうプログラムを作成し、nodeコマンドを使って実行してください

# Single Page Application (SPA)
Single Page ApplicationはモダンなWebアプリケーションを表す言葉であり、サーバサイドとの通信によって画面遷移を実施しないアプリケーションのことをいいます。

## レンダリング方法
ポイントとなるのはHTMLの出力（レンダリングも含む）をどこでどのようにおこなうのかです。以下によくある方法を記します。

### サーバサイドでHTMLをJavaなどでレンダリングする
今まで（SPAでない）のHTML出力方法で、サーバサイドのプログラムにてHTMLを組み立てます。古くはJSP、VelocityからThymeleafまでJavaにおけるHTML出力のライブラリは多岐にわたります。

### DOMを操作してJavascriptでレンダリングする
前述のDOMAPIを使って、すべての要素をレンダリングします。
そのため、DOMツリーの一部を取り替えることでメインの画面を切り替えます。

### サーバサイドでHTMLをJavascriptでレンダリングする（SSR）
クライアントサイドでDOMを操作して表示するロジックをそのままつかって、サーバサイドでDOMを作り、それをHTML出力するやり方です。
これはクライアントと同じプログラムを利用しながら、HTMLとして送信することでSEOやアクセシビリティ上の課題をクリアするものです。

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
HTMLの一部として、カスタム要素を用います。
以下の例はhttps://w3c.github.io/webcomponents/spec/custom/ から引用しています。

``` html
<flag-icon country="nl"></flag-icon>
```

flag-iconという名前の要素はHTMLにはありませんが画面コンポーネントとして定義したものを利用できるということです。

``` javascript
class FlagIcon extends HTMLElement {
  constructor() {
    super();
    this._countryCode = null;
  }

  static get observedAttributes() { return ["country"]; }

  attributeChangedCallback(name, oldValue, newValue) {
    // name will always be "country" due to observedAttributes
    this._countryCode = newValue;
    this._updateRendering();
  }
  connectedCallback() {
    this._updateRendering();
  }

  get country() {
    return this._countryCode;
  }
  set country(v) {
    this.setAttribute("country", v);
  }

  _updateRendering() {
    // Left as an exercise for the reader. But, you'll probably want to
    // check this.ownerDocument.defaultView to see if we've been
    // inserted into a document with a browsing context, and avoid
    // doing any work if not.
  }
}
```
以下の記述でこのクラスコンポーネントとflag-icon要素をマッピングします。
```javascript
customElements.define("flag-icon", FlagIcon);
```

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
ステータスが変更される動作種類（共通操作）とReducerがStoreの状態を変更するための情報を表すオブジェクト（Object型）。

```javascript
{
  type: ADD_COUNT,
  value: count
}
```

### Action Creators
コンポーネントのイベントとして呼び出される関数。上記のActionを返戻する。

以下のようにDispatchをAction Creatorで作成するパターンと
```javascript
export const increment = (count) => {
    return (dispatch) => {
      dispatch (
        {
         type: ADD_COUNT,
          value: count
        }
      )
}
```

以下のようにdispatchはさせずにmapDispatchToPropsでbindActionCreatorsを使ってDispatchで呼び出すパターンで実装が2通りある。
（React/Reduxの場合）
```javascript
export const increment = (count) => {
    return {
        type: ADD_COUNT,
        value: count
    }
}
```
```javascript
const mapDispatchToProps = (dispatch) => {
    return bindActionCreators({increment}, dispatch)
};
```

### Dispatcher
上記のActionを受け取って、新しいStoreを作成する関数。
Storeの状態は不変オブジェクトとして取り扱うのがルールであるため、新しいオブジェクトに値をコピーしています。

```javascript
export const increment_reduxer = (state = [] , action) => {

    switch (action.type) {
        case ADD_COUNT:
            return {...state,count: state.count + action.value};
        default:
            return state;
    }
}
```

### Store
SPAのアプリケーション全体が必要としている状態をすべて格納するオブジェクト（Object型）。
```javascript
import { createStore, applyMiddleware } from 'redux';

const store = createStore(
    rootReducer,
    applyMiddleware(logger)
);

export default store;
}
```
Middlewareの概念はここでは割愛します。作成したReducerをまとめるか、列挙してReduxのcreateStore関数でStoreを作り、アプリケーション全体から参照するために以下のように記載します。（React/Reduxの場合）

```javascript
ReactDOM.render(
    <Provider store={store} >
        <App />
    </Provider>
    ,
    document.getElementById('root')
);
```
このあたりの細かい解説は次回。

### mapping(Change Event + Store Query)
Storeは全体のステートとアクションを管理するため、各コンポーネントで必要な情報からみるとかなり大規模のものでわかりにくいものです。そのため、コンポーネント単位で必要なものをマッピングして、このコンポーネントから利用することを宣言します。

```javascript
const mapDispatchToProps = (dispatch) => {
    return bindActionCreators({friend}, dispatch)
};

const mapStateToProps = (state) => {
    return {data: state.findall_friend_reducer.data};
}

export default connect(mapStateToProps,mapDispatchToProps)(FriendList);
```

|関数名|説明|
|---|---|
| mapStateToProps| 状態をマッピングするメソッド（慣習）｜
|mapDispatchToProps|DispatcherとマッピングするActionCreator（慣習）|
|connect| 上記のマッピングとコンポーネントを結びつける関数|

# 画面の構造設計
* 画面のデータ構造と見栄えについて
* 画面共通と実装
* データ構造と再利用（状態の差異）
* 構成要素と粒度
詳細は次回、具体的な実装をみて、ディレクトリ構造を含めて説明します。

## 宿題3
今までのプロジェクト（設計が終わっていれば進行形でもよい）で、画面設計書をみて、画面分割をして構造的に設計するとどうなるかを検討して、画面設計書を作成してください。（一つだけでよいです）

# モダンJSの開発
モダンJS（脱jQuery)はブラウザに依存したイベント関数を列挙するという形ではなく、画面などをプログラムで作成するようなプログラミングモデルに変更されました。しかし、ブラウザがそのプログラミングモデルに対応したわけではありません。

## Web化
node.jsをベースに構築したソースコードをWebで動作させる必要があります。

### トランスパイル
プログラミングモデルにそって作ったJavascriptをブラウザで動作できるJavascriptとする必要があります。スクリプトからスクリプトに変換する動作をコンパイルするわけではないこともあり、トランスパイルと呼ばれます。

* [babel](https://babeljs.io/)

### 単一モジュール化
複数ファイル（アプリケーションコード、パッケージ）を統合し、ブラウザから容易に参照できるように単一のJSPファイルに変換します。
この変換はwebpackで実施されることが多かったですが、以下のビルドと関係しますが、各フレームワークのCLIでできるようになってきています。

* [webpack](https://webpack.js.org/)

## ビルド
従来はこれらの構成を管理して、ビルドするのが面倒でいろいろなツールが利用された。

* graunt
* gulp
* webpack

最近は各コマンドで実行される。
* [React create-react-app](https://github.com/facebook/create-react-app)
* [Angular angular-cli](https://cli.angular.io/)
* [Vue Vue CLI](https://cli.vuejs.org/)

# ES6
Nodeの書き方とES6(ECMA Script Verion 6) では若干異なります。
ReactはES6で書くことが多いため、ここで記載します。

## Classの書き方
class の記載はほぼ、Javaと一緒ですが、コンストラクタの書き方だけが異なります。

```javascript
class FriendList extends Component {
    constructor(props){
        super(props);
    }
}
```

## 関数の書き方
JSでは以下の書き方で関数を作れますが
```javascript
function sample (arg1){
  return arg1
}
```
以下の書き方がES6っぽい書き方です。（function componentで利用するのでこちらを利用するほうがよい）
```javascript
const  sample =  (arg1) => return arg1
```
仮引数の括弧は省略できる。

また、Javaとは違いJavascriptでは関数もオブジェクトであるため、関数を返す関数を作ることができます。

```javascript
    funcfunc()();

    const funcfunc = () => {
      return () => console.log('関数を戻す関数');
    }
```
みたいな記載ができます。

## Objectの取り扱い
ES6における連想配列（Object型）
リテラルで書くことができることが特長です。

```javascript
  const data = {
    key1: value1,
    key2: value2
  };
```
もちろん、これらの内容が変数であっても問題ありません。JavaでいうとMapにあたるものです。
型は特に意識することはありません。JSON形式で表現します。

書き方として以下のような書き方がよく利用されます。

``` javascript
 let {style,doc} = props
```

これは、
```javascript
  let style = props.style;
  let doc = props.doc;
```
と同じです。

## 展開記法 (スプレッド記法) 
Object型の内容を展開する基本です。Reduxではreducerで多用される記法です。

```javascript
   return {...state,count: state.count + action.value};
```
以下の書き方と同等です。ネットを見る場合はこちらの記法で書かれている場合があるため説明に加えました。
```javascript
    return Object.assign(state, {count:  state.count + action.value});
```

## Arrayで重要な関数
[リファレンス](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array)で確認しておきたい関数は以下の通りです。

|関数|説明|
|---|---|
|forEach|要素を取り出して処理をする関数|
|map|各要素に基づいて、新しい要素を作り新しい配列を返戻する関数|
|filter|条件にあう要素のみを取り出して新しい配列を返戻する関数|
|reduce/reduceRight|すべての要素を処理して一つの返戻する関数（SUMのようなもので利用）|
|every/some|要素が条件を見対しているかを判定する。booleanを返戻する関数|

## モジュールのインポート、エクスポート
詳細は[リファレンス](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Statements/import)で確認したいですが、ファイルを分けることによって、他のファイル（パッケージを含む）を参照する時にはimportが必要となります。逆に自分の作成したオブジェクト、関数、クラスを外部から参照させる場合、エクスポートが必要となります。

### デフォルトエクスポートをインポートする場合
``` javascript
import React from 'react';
```
reactモジュールのデフォルトエクスポートはReactであるため｛｝は不要。

### デフォルトエクスポート以外をインポートする場合
``` javascript
import React,{Component} from 'react';
```


