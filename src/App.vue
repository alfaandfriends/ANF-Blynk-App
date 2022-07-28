<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<script>
import router from './router'

export default {
    mounted(){
        setInterval( function(){ 
            this.checkSession();
        }.bind(this), 60000);
    },
    methods: {
        checkSession(){
            axios({
                method: 'get',
                url: 'http://127.0.0.1:8000/profile',
            })
            .then((response) => {
                console.log(response)
                if(response.data == ''){
                    router.push({name: 'login'})
                }
                this.projects = response.data.dashBoards ? response.data.dashBoards : ''
            })
            .catch((error) => {
            });
        }
    }
    
}
</script>

<template>
  <RouterView />
</template>
