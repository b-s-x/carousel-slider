<template>
  <div class="home">
    <Carousel
      class="carousel"
      :timeout="5000"
      navigation
      pagination
      startAutoPlay
      v-slot="{ currentSlide }"
    >
      <Slide
        v-for="{ id, name, extension } of carouselSlides"
        :key="id"
      >
        <div
          class="slide-info"
          v-show="currentSlide === id"
        >
          <img
            :src="require(`./assets/${name}.${extension}`)"
            alt="slide"
          >
        </div>
      </Slide>
    </Carousel>
  </div>
</template>

<script setup >
import Carousel from './components/Carousel.vue';
import Slide from './components/Slide.vue';
import { carouselSlides } from './data.js';

</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>

<style lang="scss" scoped>
.carousel {
  position: relative;
  max-height: 50vh;
  height: 50vh;
  width: 50vw;

  .slide-info {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    max-height: 100%;
    height: 100%;

    img {
      min-width: 100%;
      height: 100%;
      object-fit: cover;
    }
  }
}
</style>