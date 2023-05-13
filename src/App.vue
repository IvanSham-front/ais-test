<template>
  <main class="container">
    <rooms-el/>
  </main>
</template>

<script>
import '@/assets/styles/styles.scss'
import RoomsEl from './components/Rooms/Rooms.vue'

export default {
  name: 'App',
  components: {
    RoomsEl
  }
}
</script>
