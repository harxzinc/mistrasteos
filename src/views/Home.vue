<template>
  <div class="home">
    <NavBar />
    <Slider />
    <Quote />
  </div>
</template>

<script>

import NavBar from '@/components/NavBar.vue'
import Slider from '@/components/Slider.vue'
import Quote from '@/components/Quote.vue'

export default {
  name: 'Home',
  components: {
    NavBar,
    Slider,
    Quote
  }
}
</script>