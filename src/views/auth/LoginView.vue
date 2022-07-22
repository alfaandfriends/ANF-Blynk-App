
<template>
    <div class="rounded bg-blue-200 p-5">
        <form>
            <h3 class="text-3xl text-indigo-700 font-bold mb-5 text-center">ALFA and Friends Blynk App</h3>
            <div class="mb-3 text-slate-700 flex flex-col space-y-2">
                <label class="mb-1">Email address</label>
                <input type="email" class="rounded py-2 px-4 focus:ring-white ring-white" v-model="form.email" />
                <label class="mb-1">Password</label>
                <input type="password" class="rounded py-2 px-4" v-model="form.password" />
            </div>
            <div class="flex flex-col text-center space-y-2 mb-5">
                <button type="button" @click="submit" class="rounded bg-indigo-500 px-6 py-2 text-white hover:bg-indigo-700">Sign In</button>
                <span class="text-slate-700 font-extralight text-xs">or</span>
                <a href="/register" class="rounded bg-blue-500 px-6 py-2 text-white hover:bg-blue-700 no-underline">Register</a>
            </div>
            <div class="text-center">
                <span v-show="errorLogin" class="rounded-1 px-6 py-2 bg-red-100 text-center text-red-600">Login Error! Please try again.</span>
            </div>
 
        </form>
    </div>
</template>
 
<script>
import router from '../../router';

    export default {
        data() {
            return {
                errorLogin: false,
                form: {
                    email: "",
                    password: "",
                }
            }
        },
        methods: {
            submit(){
                axios({
                    method: 'get',
                    url: 'http://127.0.0.1:8000/login/' + this.form.email + '/' + this.form.password,
                })
                .then(function (response) {
                    console.log(response)
                    if(response.data == 'OK'){
                        router.push({name: 'dashboard'})
                    }
                })
                .catch(function (error) {
                    this.errorLogin = true
                });
            },
            register(){

            }
        }
    }
</script>