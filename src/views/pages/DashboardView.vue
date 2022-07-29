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
                    <button class="flex py-2 px-6 btn bg-indigo-500 hover:bg-indigo-600 text-white rounded items-center" data-bs-toggle="modal" data-bs-target="#exampleModal">
                        <svg class="w-4 h-4 fill-current opacity-50 shrink-0" viewBox="0 0 16 16">
                        <path d="M15 7H9V1c0-.6-.4-1-1-1S7 .4 7 1v6H1c-.6 0-1 .4-1 1s.4 1 1 1h6v6c0 .6.4 1 1 1s1-.4 1-1V9h6c.6 0 1-.4 1-1s-.4-1-1-1z"></path>
                        </svg>
                        <span class="ml-2">Add Project</span>
                    </button>
                </div>
                <div class="grid grid-cols-12 gap-6">
                    <div class="flex flex-col col-span-full sm:col-span-6 xl:col-span-3 bg-white shadow-lg rounded-sm border border-gray-200 transform transition hover:scale-105" v-for="(project, index) in projects" :key="index">
                        <div class="px-5 py-5" @click="openProject(project.id)">
                            <h2 class="text-lg font-semibold text-blue-800 mb-2">{{ project.name }}</h2>
                            <div class="flex space-x-4">
                                <div class="flex flex-col">
                                    <div class="text-xs font-semibold text-indigo-400 uppercase mb-1">Authentication Token</div>
                                    <div class="flex items-start">
                                        <div class="font-bold text-gray-500 mr-2">{{ project.devices ? project.devices[0].token : 0 }}</div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
             <!-- Modal -->
            <div class="modal fade fixed top-0 left-0 hidden w-full h-full outline-none overflow-x-hidden overflow-y-auto" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
                <div class="modal-dialog relative w-auto pointer-events-none">
                    <div class="modal-content border-none shadow-lg relative flex flex-col w-full pointer-events-auto bg-white bg-clip-padding rounded-md outline-none text-current">
                        <div
                            class="modal-header flex flex-shrink-0 items-center justify-between p-4 border-b border-gray-200 rounded-t-md">
                            <h5 class="text-xl font-medium leading-normal text-gray-800" id="exampleModalLabel">Create New Project</h5>
                            <button type="button"
                            class="btn-close box-content w-4 h-4 p-1 text-black border-none rounded-none opacity-50 focus:shadow-none focus:outline-none focus:opacity-100 hover:text-black hover:opacity-75 hover:no-underline"
                            data-bs-dismiss="modal" aria-label="Close"></button>
                        </div>
                        <div class="modal-body relative p-4">
                            <div class="flex flex-col space-y-2 text-slate-800">
                                <label for="project_name" class="">Project Name</label>
                                <input type="text" class="border rounded py-2 px-4 ring-0 ring-transparent focus:ring-0 focus:ring-transparent" v-model="project_name">
                            </div>
                        </div>
                        <div class="modal-footer flex flex-shrink-0 flex-wrap items-center justify-end p-4 border-t border-gray-200 rounded-b-md">
                            <button type="button" class="px-6
                                py-2.5
                                bg-gray-600
                                text-white
                                font-medium
                                text-xs
                                leading-tight
                                uppercase
                                rounded
                                shadow-md
                                hover:bg-gray-700 hover:shadow-lg
                                focus:bg-gray-700 focus:shadow-lg focus:outline-none focus:ring-0
                                active:bg-gray-800 active:shadow-lg
                                transition
                                duration-150
                                ease-in-out" data-bs-dismiss="modal">Cancel
                            </button>
                            <button @click="createProject" type="button" class="px-6
                                py-2.5
                                bg-blue-600
                                text-white
                                font-medium
                                text-xs
                                leading-tight
                                uppercase
                                rounded
                                shadow-md
                                hover:bg-blue-700 hover:shadow-lg
                                focus:bg-blue-700 focus:shadow-lg focus:outline-none focus:ring-0
                                active:bg-blue-800 active:shadow-lg
                                transition
                                duration-150
                                ease-in-out
                                ml-1">Create Project
                            </button>
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
  data(){
    return{
        project_name: '',
        projects: ''
    }
  },
  mounted(){
        this.checkSession()
        setInterval( function(){ 
            this.checkSession()
        }.bind(this), 10000);
  },
  methods: {
    createProject(){
        axios({
            method: 'get',
            url: 'http://alfaandfriends.tplinkdns.com:8000/createDash/' + (this.projects != '' ? (this.projects.length + 1) : 1) + '/' + this.project_name,
        })
        .then((response) => {
            console.log(response)
            if(response.status == '200'){
                router.go(0)
            }
        })
        .catch((error) => {
        });
    },
    openProject(projectId){
        axios({
            method: 'get',
            url: 'http://alfaandfriends.tplinkdns.com:8000/activateDash/' + projectId,
        })
        .then((response) => {
            if(response.status == '200'){
                router.push({name: 'project', params: { projectId: projectId }})
            }
        })
        .catch((error) => {
        });
    },
    checkSession(){
        axios({
            method: 'get',
            url: 'http://alfaandfriends.tplinkdns.com:8000/profile',
            timeout: 30000,
        })
        .then((response) => {
            this.projects = response.data.dashBoards ? response.data.dashBoards : ''
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