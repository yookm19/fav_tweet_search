# fav-tweet-search

> A Vue.js project

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build
```

For detailed explanation on how things work, consult the [docs for vue-loader](http://vuejs.github.io/vue-loader).

ツイッターから、ログインユーザーが直近でいいねしたツイートを取得するアプリ。

【処理概要】  
フロントをVue.jsで実装し、サーバーサイドをpythonで実装。  
ログイン成功後、検索画面を表示し、ボタンを押下するとツイートを検索する。  
ログイン時に取得したユーザー情報からuidを取得し、サーバーサイドへ渡す。  

【困っていること】  
フロントからサーバーサイドへデータを渡すところで接続エラーが発生。  
CORSエラーなるもの。  
https://qiita.com/narutaro/items/26e4cbec386bc054bbd7  
クライアントとサーバー間の通信で発生するエラーらしい。  
異なるドメインのリソースにアクセスしようとすると、  
サーバー側のレスポンスで拒否されるらし。  
クライアントではなくサーバーを守るための仕組みとのこと。  
しらんけど。。  
  
いまは、firebaseにデプロイしたクライアントと、  
まだローカルにいるサーバープログラムとの間で発生していると思われる。  
「Access-Control-Allow-Origin:*」ていうのをサーバー側に設定すると、  
クロスドメインのアクセスを許可できるらしいが、設定方法がよくわからない。。  