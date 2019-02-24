<template>
  <div id="app">
    <Home v-if="!isLogin"></Home>
    <Search v-if="isLogin" :user="userData"></Search>
  </div>
</template>

<script>
import Home from "./components/Home.vue";
import Search from "./components/Search.vue";

export default {
  name: 'app',
  data () {
    return {
      isLogin:false,
      userData:null
    }
  },
  created:function(){
    firebase.auth().onAuthStateChanged(user => {
      console.log(user);
    if(user){
      this.isLogin = true;
      this.userData = user;
    }else{
      this.isLogin = false;
      this.userData = null;
    }
    })
  },
  components:{
    Home:Home,
    Search:Search
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
} 

.image{
  width:10%;
  height:10%;
}

.count{
  text-align:left;
  padding:10px 10px 0px 50px;
  position:relative;
}

[v-cloak]{
  display:none;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  text-align:left;
  position:relative;
}

li {
  display: block;
  margin: 0 10px;
  padding:10px 10px 0px 0px;
  background: #f1f8ff;
  border-bottom: solid 2px #dadada;
  border-left: solid 6px #2d8fdd;
  margin-bottom: 3px;
  line-height: 1.5;
  padding: 0.5em;
  border-radius: 0 15px 15px 0;
}

a {
  color: #42b983;
}

button {
  display: inline-block;
  padding: 0.5em 1em;
  text-decoration: none;
  background: #f1f8ff;/*ボタン色*/
  color: #000000;
  border-bottom: solid 4px #627295;
  border-radius: 15px 15px 15px 15px;
}
button:active {
  /*ボタンを押したとき*/
  -webkit-transform: translateY(4px);
  transform: translateY(4px);/*下に動く*/
  border-bottom: none;/*線を消す*/
}

</style>
