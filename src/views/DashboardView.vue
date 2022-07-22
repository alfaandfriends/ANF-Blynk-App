
<template>
    <div>
        <p class="text-green-500">Login Success</p>
    </div>
</template>

<script>
import router from '../router';
export default {
    data(){
        return {

        }
    },
    created(){
        axios({
            method: 'get',
            url: 'http://127.0.0.1:8000/profile',
        })
        .then((response) => {
            console.log(response)
            // if(!response.data.length){
            //     router.push({name: 'login'})
            // }
        })
        .catch((error) => {
            // this.errorLogin = true
        });
    }
}
</script>