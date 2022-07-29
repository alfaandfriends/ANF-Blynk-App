<template>
    <div class="flex h-screen overflow-hidden">

    <!-- Sidebar -->
    <Sidebar :sidebarOpen="sidebarOpen" @close-sidebar="sidebarOpen = false" />

    <!-- Content area -->
    <div class="relative flex flex-col flex-1 overflow-y-auto overflow-x-hidden">
      
        <!-- Site header -->
        <Header :sidebarOpen="sidebarOpen" @toggle-sidebar="sidebarOpen = !sidebarOpen" />

        <main>
            <div class="px-4 sm:px-6 lg:px-8 py-4 sm:py-8 md:py-8 w-full max-w-9xl mx-auto">
            
                <!-- Dashboard actions -->
                <div class="sm:flex sm:justify-between sm:items-center mb-4">
                    <button class="flex py-2 px-6 btn bg-blue-500 hover:bg-blue-600 text-white rounded items-center" @click="runProject()" v-if="project_status == false">
                        <svg xmlns="http://www.w3.org/2000/svg" class="text-white" height="15" width="15" viewBox="0 0 448 512"><!-- Font Awesome Pro 5.15.4 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) --><path d="M424.4 214.7L72.4 6.6C43.8-10.3 0 6.1 0 47.9V464c0 37.5 40.7 60.1 72.4 41.3l352-208c31.4-18.5 31.5-64.1 0-82.6z"/></svg>
                        <span class="ml-2">Run Project</span>
                    </button>
                    <button class="flex py-2 px-6 btn bg-red-500 hover:bg-red-600 text-white rounded items-center" @click="runProject()" v-if="project_status == true">
                        <svg xmlns="http://www.w3.org/2000/svg" class="text-white" height="15" width="15" viewBox="0 0 448 512"><!-- Font Awesome Pro 5.15.4 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) --><path d="M400 32H48C21.5 32 0 53.5 0 80v352c0 26.5 21.5 48 48 48h352c26.5 0 48-21.5 48-48V80c0-26.5-21.5-48-48-48z"/></svg>
                        <span class="ml-2">Stop Project</span>
                    </button>
                </div>
                <div class="grid grid-cols-12 gap-6">
                    <div class="flex flex-col col-span-full sm:col-span-6 xl:col-span-2 bg-white shadow-lg rounded-sm border border-gray-200" v-for="(widget, index) in widgets" :key="index">
                        <div class="px-5 py-5" v-if="widget.type == 'BUTTON'">
                            <div class="flex items-center justify-between">
                                <h2 class="text-lg font-semibold text-blue-800">{{ widget.label }}</h2>
                                <div class="flex flex-col">
                                    <div class="flex items-start">
                                        <div class="font-bold text-gray-500 mr-2">
                                            <div class="flex justify-center">
                                                <div class="form-check form-switch">
                                                    <input class="form-check-input appearance-none w-16 -ml-10 rounded-full float-left h-7 align-top bg-no-repeat bg-contain bg-gray-300 focus:outline-none cursor-pointer shadow-sm transform transition hover:scale-105" type="checkbox" role="switch" id="flexSwitchCheckDefault" v-model="button_status" @click="buttonSwitchAction" :disabled="button_disabled">
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </main>
    </div> 

  </div>
</template>


<script>
import { ref } from 'vue'
import Sidebar from '../partials/Sidebar.vue'
import Header from '../partials/Header.vue'
import router from '../../router';

export default {
    name: 'Dashboard',
    components: {
        Sidebar,
        Header
    },
    props: {
        projectId: String
    },
    data(){
        return{
            button_status: false,
            button_disabled: true,
            project_status: false,
            projects: '',
            widgets: ''
        }
    },
    mounted(){
        this.checkSession()
        axios({
            method: 'get',
            url: 'http://alfaandfriends.tplinkdns.com:8000/deactivateDash/'+this.projectId,
        })
        .then((response) => {
        })
        .catch((error) => {
        });
        setInterval( function(){ 
            this.checkSession()
        }.bind(this), 10000);
    },
    methods: {
        runProject(){
            if(this.project_status == false){
                axios({
                    method: 'get',
                    url: 'http://alfaandfriends.tplinkdns.com:8000/activateDash/'+this.projectId,
                })
                .then((response) => {
                    if(response.data == "7"){
                        alert('Device not in network')
                    }
                    else{
                        this.project_status=true
                        this.button_disabled=false
                    }
                })
                .catch((error) => {
                });
            }
            else{
                axios({
                    method: 'get',
                    url: 'http://alfaandfriends.tplinkdns.com:8000/deactivateDash/'+this.projectId,
                })
                .then((response) => {
                    this.project_status=false
                    this.button_disabled=true
                })
                .catch((error) => {
                });
            }
        },
        buttonSwitchAction(){
            if(this.button_status == false){
                axios({
                    method: 'get',
                    url: 'http://alfaandfriends.tplinkdns.com:8000/hardware/' + this.projectId + '-0/vw/0/1',
                    timeout: 100,
                })
                .then((response) => {
                    this.button_status = true
                })
                .catch((error) => {
                });
            }
            else{
                axios({
                    method: 'get',
                    url: 'http://alfaandfriends.tplinkdns.com:8000/hardware/' + this.projectId + '-0/vw/0/0',
                    timeout: 100,
                })
                .then((response) => {
                    this.button_status = false
                })
                .catch((error) => {
                });
            }
        },
        checkSession(){
            axios({
                method: 'get',
                url: 'http://alfaandfriends.tplinkdns.com:8000/profile',
                timeout: 30000,
            })
            .then((response) => {
                this.projects = response.data.dashBoards ? response.data.dashBoards : ''
                if(!this.projects[this.projectId-1]){
                    router.push({name: 'dashboard'})
                }

                this.widgets = this.projects[this.projectId-1].widgets ? this.projects[this.projectId-1].widgets : ''
            })
            .catch((error) => {
                router.push({name: 'login'})
            });
        }
    },
    setup() {

        const sidebarOpen = ref(false)

        return {
        sidebarOpen,
        }  
    },
}
</script>