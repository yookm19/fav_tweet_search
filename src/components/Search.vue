<template>
    <div id="search">
        <div class="image"><img :src="user.photoURL"></div>
        <h1>{{ user.displayName }}さんが最近いいねしたツイートを探します</h1>
        <span>{{ user.displayName }}さんの</span>
        <button v-on:click="searchTweet">いいねサーチ</button>
        <input type="text" v-model="keyword">
        <p>
            <ul>
                <li v-for="searchTweet in searchTweets">
                    <a>{{ searchTweet }}</a>
                    <!-- <pre>{{ searchTweet }}</pre> -->
                    <!-- <a v-bind:href="tweet.url" target="_blank">{{ tweet }}</a> -->
                </li>
            </ul>
        </p>
        <p>
            <button v-on:click="logout">ログアウト</button>
        </p>
    </div>
</template>
<style>
</style>

<script>

export default{
    name:"search",
    props:["user"],
    data(){
        return{
            message:'',
            keyword:"",         // 検索用キーワード文字列
            orgTweets:"",       // サーバーサイドから取得した元データ
            tweets:[],          // 元データをタブ区切りで格納する配列
            searchTweets:[]     // キーワード検索結果を格納する配列(ビュー表示はこれを使う)
        };
    },
    watch:{
        keyword:function(newKeyword,oldKeyword){
            this.debouncedGetAnswer()          // keywordプロパティを監視
        }
    },
    created:function(){
        this.debouncedGetAnswer = _.debounce(this.keywordSearch,1000)       // 監視プロパティに一定時間変化がなければ検索を実行
    },
    methods:{
        // ログアウト
        logout:function(){
            firebase.auth().signOut();
        },
        //#region  サーバーサイドにアクセスしてツイートを取得する。
        // axios引数
        // path：サーバーモジュールの場所
        // uid：ツイートを取得するユーザーのuid
        // count：いいねツイートを取得するツイート件数(ツイート日時順に上位n件)
        searchTweet:function(){
            // 配列初期化
            this.orgTweets=""
            this.tweets.splice(0,this.tweets.length)
            this.searchTweets.splice(0,this.searchTweets.length)

            const path = 'http://127.0.0.1:5000/'
            var vm = this
            axios.get(path,
                    {
                        params:{
                            uid:this.user.providerData[0].uid,
                            count:100
                        }
                    }
                )
                .then(function(response){
                    // サーバーサイドから一連のツイートをタブ区切りで文字列データとして取得
                    vm.orgTweets = response.data
                })
                .catch(function(error){
                    // ↓のコメントに見える一行は、コメントではないらしいので削除しない。
                    // eslint-disable-next-line
                    console.log(error)
                })
                .finally(function(){
                    // 文字列データをタブ区切りで配列に格納
                    vm.tweets = vm.orgTweets.split("    ")
                    // キーワード検索表示用配列にも初期表示用に格納
                    for(var i = 0; i<vm.tweets.length; i++){
                        vm.searchTweets.push(
                            vm.tweets[i]
                        )
                    }
                })
        },
        //#endregion サーバーサイドにアクセスしてツイートを取得する。

         //#region  お気に入りツイートのキーワード検索
        keywordSearch:function(){
            // いったん配列を空にする
            this.searchTweets.splice(0,this.searchTweets.length)
            // 取得ツイート全件に対してキーワード検索
            for(var i = 0; i < this.tweets.length; i++){
                var tweet = this.tweets[i]
                if(this.keyword === ""){
                    // キーワード指定がなければ全件表示
                    this.searchTweets.push(this.tweets[i])
                }
                else if(this.keyword !== "" && tweet.indexOf(this.keyword) !== -1){
                    // キーワードに一致するツイートのみ表示
                    this.searchTweets.push(this.tweets[i])
                }
            }
        }
        //#endregion お気に入りツイートのキーワード検索
    }
};
</script>
