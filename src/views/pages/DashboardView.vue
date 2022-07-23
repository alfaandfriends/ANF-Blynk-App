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
                    <button class="flex py-2 px-6 btn bg-indigo-500 hover:bg-indigo-600 text-white rounded items-center">
                    <svg class="w-4 h-4 fill-current opacity-50 shrink-0" viewBox="0 0 16 16">
                    <path d="M15 7H9V1c0-.6-.4-1-1-1S7 .4 7 1v6H1c-.6 0-1 .4-1 1s.4 1 1 1h6v6c0 .6.4 1 1 1s1-.4 1-1V9h6c.6 0 1-.4 1-1s-.4-1-1-1z"></path>
                    </svg>
                    <span class="ml-2">Add Project</span>
                    </button>
                </div>
                <div class="grid grid-cols-12 gap-6">
                    <div class="flex flex-col col-span-full sm:col-span-6 xl:col-span-4 bg-white shadow-lg rounded-sm border border-gray-200 transform transition duration-500 hover:scale-105">
                        <div class="px-5 py-5">
                            <h2 class="text-lg font-semibold text-gray-800 mb-2">Acme Plus</h2>
                            <div class="text-xs font-semibold text-gray-400 uppercase mb-1">Sales</div>
                            <div class="flex items-start">
                                <div class="text-3xl font-bold text-gray-800 mr-2">$24,780</div>
                                <div class="text-sm font-semibold text-white px-1.5 bg-green-500 rounded-full">+49%</div>
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
    Header,
  },
  mounted(){
        axios({
            method: 'get',
            url: 'http://127.0.0.1:8000/profile',
        })
        .then((response) => {
            console.log(response)
            if(response.data == ''){
                router.push({name: 'login'})
            }
        })
        .catch((error) => {
        });
  },
  setup() {

    const sidebarOpen = ref(false)

    return {
      sidebarOpen,
    }  
  },
}
</script>