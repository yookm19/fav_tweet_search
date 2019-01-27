<template>
    <div id="search">
        <div class="image"><img :src="user.photoURL"></div>
        <h1>{{ user.displayName }}さんが最近いいねしたツイートを探します</h1>
        <span>{{ user.displayName }}さんの</span>
        <button v-on:click="search">いいねサーチ</button>
        <!-- <p>
            <input type="text" v-model="keyword">
        </p>
        <p>
          {{message}}
        </p>
        <ul>
          <li v-for="item in items"> -->
            <!-- {{item.title}} -->
            <!-- <a v-bind:href="item.url" target="_blank">{{item.title}}</a> likes:{{item.likes_count}}
          </li>
        </ul> -->
        <p>
            <button v-on:click="logout">ログアウト</button>
        </p>
    </div>
</template>

<script>
export default{
    name:"search",
    props:["user"],
    data(){
        return{
            message:'',
            keyword:"Javascript"
        };
    },
    methods:{
        logout:function(){
            firebase.auth().signOut();
        },
        search:function(){
        //     var vm=this
        //     var params={user_id:"@yesmisteryesman",count:20}
        //     axios.get('https://api.twitter.com/1.1/favorites/list')
        //     .then(function(response){
        //         console.log(response)
        //         //vm.items=response.data
        //     })
        //     .catch(function(error){
        //         vm.message='Error!' + error
        //     })
        //     .finally(function(){
        //         vm.message=''
        //     })

            client.get('statuses/home_timeline.json',params,(error,tweets,response)=>{
            if(!error){
                console.log(tweets)
            }
        })
        }
    }
};
</script>