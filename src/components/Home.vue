<template>
    <div id="home">
        <h1>{{ title }}</h1>
        <h2>{{ header }}</h2>
        <h3>{{ explanation }}</h3>
        <button v-on:click="twitterLogin">Twitterアカウントでログイン</button>
    </div>
</template>

<script>
export default{
    name:"home",
    data(){
        return{
            title:"Welcome to Fav-Tweet-Search",
            header:"あなたが最近いいねしたツイートを探してきます。",
            explanation:"Twitterアカウントでログインしてみてください。"
        };
    },
    methods:{
        twitterLogin:function(){
            firebase
                .auth()
                    .signInWithRedirect(new firebase.auth.TwitterAuthProvider());
        },
    }
};
</script>