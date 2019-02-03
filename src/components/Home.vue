<template>
    <div id="home">
        <h1>{{ msg }}</h1>
        <button v-on:click="twitterLogin">Twitterアカウントでログイン</button>
        <!-- 以下、テストコード -->
        <!-- <button v-on:click="searchTweet">テスト</button>
        <pre>{{ tweets }}</pre> -->
        <!-- 以上、テストコード -->
    </div>
</template>

<script>
export default{
    name:"home",
    data(){
        return{
            msg:"Welcome to Fav-Tweet-Search",
            tweets:null
        };
    },
    methods:{
        twitterLogin:function(){
            firebase
                .auth()
                    .signInWithRedirect(new firebase.auth.TwitterAuthProvider());
        },
        // // 以下、テストコード
        // searchTweet:function(){
        //     const path = 'http://127.0.0.1:5000/'
        //     axios.get(path)
        //         .then(function(response){
        //             console.log(response)
        //             this.tweets = response.data
        //         })
        //         .catch(function(error){
        //             // eslint-disable-next-line
        //             console.log(error)
        //         })
        //         .finally(function(){
        //             // なんか書く
        //         })
        // }
        // // 以上、テストコード
    }
};
</script>