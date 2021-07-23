<template>
  <div class="home">
    <NavBar />
    <div class="w-100">
      <div
        class="container containermain shadow-lg p-3 mb-5 bg-body rounded mt-5"
      >
        <Slider />
        <Quote />
      </div>
    </div>
    <Footer />
  </div>
</template>

<script>
import NavBar from "@/components/NavBar.vue";
import Slider from "@/components/Slider.vue";
import Quote from "@/components/Quote.vue";
import Footer from "@/components/Footer.vue";

export default {
  name: "Home",
  components: {
    NavBar,
    Slider,
    Quote,
    Footer
  },
};
</script>

<style scoped>
@media (min-width: 700px) {
  .containermain {
    max-width: 700px;
  }
}
</style>