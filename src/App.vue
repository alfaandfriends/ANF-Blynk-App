<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<script>
import router from './router'

export default {
    mounted(){
        setInterval( function(){ 
            this.checkSession();
        }.bind(this), 10000);
    },
    methods: {
        checkSession(){
            axios({
                method: 'get',
                url: 'http://alfaandfriends.tplinkdns.com:8000/profile',
                timeout: 1000,
            })
            .then((response) => {
                // console.log(response)
                // if(response.data == ''){
                //     router.push({name: 'login'})
                // }
                this.projects = response.data.dashBoards ? response.data.dashBoards : ''
            })
            .catch((error) => {
                    router.push({name: 'login'})
            });
        }
    }
    
}
</script>

<template>
  <RouterView />
</template>
