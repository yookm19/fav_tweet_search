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
            keyword:"",
            orgTweets:"",
            tweetArray:null,
            tweets:[
            ],
            searchTweets:[

            ]
        };
    },
    watch:{
        keyword:function(newKeyword,oldKeyword){
            // console.log(newKeyword,oldKeyword)
            this.debouncedGetAnswer()
        }
    },
    created:function(){
        this.debouncedGetAnswer = _.debounce(this.keywordSearch,1000)
    },
    methods:{
        logout:function(){
            firebase.auth().signOut();
        },
        searchTweet:function(){
            // サーバーサイドにアクセスしてツイートを取得する。
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
                    // console.log(response.data)
                    vm.orgTweets = response.data
                })
                .catch(function(error){
                    // ↓のコメントに見える一行は、コメントではないらしいので削除しない。
                    // eslint-disable-next-line
                    console.log(error)
                })
                .finally(function(){
                    // なんか書く
                    vm.tweetArray = vm.orgTweets.split("    ")
                    for(var i = 0; i<vm.tweetArray.length; i++){
                        vm.tweets.push(
                            vm.tweetArray[i]
                        )
                        vm.searchTweets.push(
                            vm.tweetArray[i]
                        )
                    }
                })
        },
        keywordSearch:function(){
            this.searchTweets.splice(0,this.searchTweets.length)
            for(var i = 0; i < this.tweets.length; i++){
                var tweet = this.tweets[i]
                if(this.keyword === ""){
                    this.searchTweets.push(this.tweets[i])
                }
                else if(this.keyword !== "" && tweet.indexOf(this.keyword) !== -1){
                    console.log(tweet)
                    this.searchTweets.push(this.tweets[i])
                }
            }
        }
    }
};
</script>

<style lang="scss">
a {
  color: #42b983;
  text-align: left;
}
</style>