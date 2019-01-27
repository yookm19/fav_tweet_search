<template>
    <div id="home">
        <h1>{{ msg }}</h1>
        <button v-on:click="twitterLogin">Twitterアカウントでログイン</button>
    </div>
</template>

<script>
export default{
    name:"home",
    data(){
        return{
            msg:"Welcome to Fav-Tweet-Search"
        };
    },
    methods:{
        twitterLogin:function(){
            firebase
                .auth()
                    .signInWithRedirect(new firebase.auth.TwitterAuthProvider());
        }
    }
};
</script>