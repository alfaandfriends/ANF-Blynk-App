<template>
  <div class="relative inline-flex">
    <button
      ref="trigger"
      class="inline-flex justify-center group place-items-end"
      aria-haspopup="true"
      @click.prevent="dropdownOpen = !dropdownOpen"
      :aria-expanded="dropdownOpen"
    >
      <svg version="1.1" width="25" height="25" id="Capa_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px"
        viewBox="0 0 502.664 502.664" style="enable-background:new 0 0 502.664 502.664;" xml:space="preserve">
        <path style="fill:#010002;" d="M251.364,288.229c52.266,0,94.631-42.365,94.631-94.631s-42.365-94.631-94.631-94.631
          s-94.631,42.365-94.631,94.631S199.12,288.229,251.364,288.229z M281.714,121.379c14.819,0,26.812,12.015,26.812,26.812
          s-12.015,26.791-26.812,26.791s-26.812-11.972-26.812-26.791C254.902,133.372,266.895,121.379,281.714,121.379z"/>
        <path style="fill:#010002;" d="M309.174,315.839H205.419c-36.519,0-66.416,29.876-66.416,66.438v120.387h236.653V382.277
          C375.634,345.736,345.715,315.839,309.174,315.839z"/>
        <path style="fill:#010002;" d="M143.187,110.184l-41.934-41.977l17.817-17.839l41.977,41.955L143.187,110.184z M393.602,180.655
          v25.238h59.32v-25.238H393.602z M49.742,205.893h59.341v-25.238H49.742V205.893z M401.475,68.207l-17.861-17.839l-41.998,41.955
          l17.861,17.861L401.475,68.207z M267.025,0h-27.33v55.825h27.33V0z"/>
      </svg>
      <div class="flex items-center truncate">
        <span class="truncate ml-2 text-sm font-medium group-hover:text-gray-800">Welcome, User</span>
        <svg class="w-3 h-3 shrink-0 ml-1 fill-current text-gray-400" viewBox="0 0 12 12">
          <path d="M5.9 11.4L.5 6l1.4-1.4 4 4 4-4L11.3 6z" />
        </svg>
      </div>
    </button>
    <transition
      enter-active-class="transition ease-out duration-200 transform"
      enter-from-class="opacity-0 -translate-y-2"
      enter-to-class="opacity-100 translate-y-0"
      leave-active-class="transition ease-out duration-200"
      leave-from-class="opacity-100"
      leave-to-class="opacity-0"
    >
      <div v-show="dropdownOpen" class="origin-top-right z-10 absolute top-full min-w-44 bg-white border border-gray-200 py-1.5 rounded shadow-lg overflow-hidden mt-1" :class="align === 'right' ? 'right-0' : 'left-0'">
        <ul
          ref="dropdown"
          @focusin="dropdownOpen = true"
          @focusout="dropdownOpen = false"
        >
          <!-- <li>
            <router-link class="font-medium text-sm text-indigo-500 hover:text-indigo-600 flex items-center py-1 px-3" to="/" @click="dropdownOpen = false">Settings</router-link>
          </li> -->
          <li>
            <button class="font-medium text-sm text-indigo-500 hover:text-indigo-600 flex items-center py-1 px-3" @click="logout()">Sign Out</button>
          </li>
        </ul>
      </div> 
    </transition>
  </div>
</template>

<script>
import { ref, onMounted, onUnmounted } from 'vue'
import UserAvatar from '../images/user-avatar-32.png'
import router from '../../router';

export default {
  name: 'DropdownProfile',
  props: ['align'],
  data() {
    return {
      UserAvatar: UserAvatar,
    }
  },  
  setup() {

    const dropdownOpen = ref(false)
    const trigger = ref(null)
    const dropdown = ref(null)

    // close on click outside
    const clickHandler = ({ target }) => {
      if (!dropdownOpen.value || dropdown.value.contains(target) || trigger.value.contains(target)) return
      dropdownOpen.value = false
    }

    // close if the esc key is pressed
    const keyHandler = ({ keyCode }) => {
      if (!dropdownOpen.value || keyCode !== 27) return
      dropdownOpen.value = false
    }

    onMounted(() => {
      document.addEventListener('click', clickHandler)
      document.addEventListener('keydown', keyHandler)
    })

    onUnmounted(() => {
      document.removeEventListener('click', clickHandler)
      document.removeEventListener('keydown', keyHandler)
    })

    return {
      dropdownOpen,
      trigger,
      dropdown,
    }
  },
  methods: {
    logout(){
        axios({
            method: 'get',
            url: 'http://127.0.0.1:8000/logout',
        })
        .then((response) => {
          router.push({name: 'login'})
        })
        .catch((error) => {
        });
    }
  }
}
</script>