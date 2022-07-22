
<template>
    <div class="rounded p-8" style="background-color: #a8dadc;">
        <form>
            <h3 class="text-3xl font-bold mb-5 text-center" style="color: #1d3557">ALFA and Friends Blynk App</h3>
            <h3 class="text-lg text-center mb-2" style="color: #457b9d">Sign In</h3>
            <div class="mb-3 text-slate-700 flex flex-col space-y-2">
                <label class="mb-1" style="color: #1d3557">Email address</label>
                <input type="email" class="rounded py-2 px-4 focus:ring-white ring-white" v-model="form.email" />
                <label class="mb-1" style="color: #1d3557">Password</label>
                <input type="password" class="rounded py-2 px-4" v-model="form.password" />
            </div>
            <div class="flex flex-col text-center space-y-3  mb-5">
                <button type="button" @click="submit" class="text-sm rounded bg-indigo-500 px-6 py-2 text-white hover:bg-indigo-700">Sign in</button>
                <span class="text-slate-700 font-extralight text-xs">or</span>
                <a href="/register" class="text-sm rounded bg-slate-700 px-6 py-2 text-white hover:bg-gray-600 no-underline">Create New Account</a>
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
        // mounted(){
        //     axios({
        //         method: 'get',
        //         url: 'http://127.0.0.1:8000/profile',
        //     })
        //     .then((response) => {
        //         console.log(response)
        //         if(response.data){
        //             router.push({name: 'dashboard'})
        //         }
        //     })
        //         .catch((error) => {
        //             router.push({name: 'login'})
        //         });
        // },
        data() {
            return {
                errorLogin: false,
                form: {
                    email: "",
                    password: "",
                }
            }
        },
        created(){
            // this.$storage.setStorageSync("test-key", "testdata");
        },
        methods: {
            submit(){
                axios({
                    method: 'get',
                    url: 'http://127.0.0.1:8000/login/' + this.form.email + '/' + this.form.password,
                })
                .then((response) => {
                    console.log(response)
                    if(response.data == '200'){
                        router.push({name: 'dashboard'})
                    }
                    else{
                        this.errorLogin = true
                    }
                })
                .catch((error) => {
                    // router.push({name: 'login'})
                    this.errorLogin = true
                });
            },
            register(){

            }
        }
    }
</script>