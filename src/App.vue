<template>
    <Login v-if="!isLoggedIn" @isValidLogin="isValidLogin()"/>
    <Home v-else/>
</template>

<script>
import Home from './components/Home.vue'
import Login from './components/Login.vue'

export default {
    name: 'App',
    components: {
        Home,
        Login
    },
    methods: {
        isValidLogin: function(){
            this.isLoggedIn= true;
        }
    },
    data() {
        return {
            isLoggedIn: false
        }
    }

}


</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
