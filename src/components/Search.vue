<template>
    <div id="search">
        <div class="image"><img :src="user.photoURL"></div>
        <h1>{{ user.displayName }}さんが最近いいねしたツイートを探します</h1>
        <span>{{ user.displayName }}さんの</span>
        <button v-on:click="searchTweet">いいねサーチ</button>
        <p>{{ tweets }}
            <ul>
                <li v-for="tweet in tweets">
                    {{ tweet }}
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
import Timeline from 'vue-tweet-embed'

export default{
    name:"search",
    props:["user"],
    data(){
        return{
            message:'',
            keyword:"Javascript",
            tweets:null
        };
    },
    methods:{
        logout:function(){
            firebase.auth().signOut();
        },
        searchTweet:function(){
            // サーバーサイドにアクセスしてツイートを取得する。
            // ここのサーバーサイド呼び出し時にエラーが発生している。
            const path = 'http://127.0.0.1:5000/'
            // const path = '/flask_get_favorites/get_favorites.py'
            var param = this.user.providerData[0].uid
            console.log(param)
            var vm = this
            axios.get(path,
                    {
                        params:{
                            uid:this.user.providerData[0].uid,
                            count:200
                        }
                    }
                )
                .then(function(response){
                    console.log(response)
                    console.log(response.data)
                    console.log(response.request.responseText)
                    vm.tweets = response.data
                })
                .catch(function(error){
                    // ↓のコメントに見える一行は、コメントではないらしいので削除しない。
                    // eslint-disable-next-line
                    console.log(error)
                })
                .finally(function(){
                    // なんか書く
                })
        }
    }
};
</script>