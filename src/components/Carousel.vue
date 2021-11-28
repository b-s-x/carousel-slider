<template>
  <div class="carousel">
    <slot :currentSlide="currentSlide" />

    <div
      class="navigate"
      v-if="navEnabled"
    >
      <div class="toggle-page left">
        <i
          class="fas fa-chevron-left"
          @click="prevSlide"
        />
      </div>
      <div class="toggle-page right">
        <i
          class="fas fa-chevron-right"
          @click="nextSlide"
        />
      </div>
    </div>

    <div
      class="pagination"
      v-if="paginationEnabled"
    >
      <span
        v-for="(slide, index) of getSlideCount"
        :key="index"
        :class="{active: index + 1 === currentSlide }"
        @click="goToSlide(index)"
      />
    </div>
  </div>
</template>

<script setup>
  import { ref, onMounted } from 'vue'

  const props = defineProps({
    startAutoPlay: {
      type: Boolean,
      default: false,
    },
    timeout: {
      type: Number,
      default: 0,
    },
    navigation: {
      type: Boolean,
      default: false,
    },
    pagination: {
      type: Boolean,
      default: true,
    },
  })

  const currentSlide = ref(1);
  const getSlideCount = ref(null);
  const autoPlayEnabled = ref(props.startAutoPlay === undefined ? true : props.startAutoPlay);
  const timeoutDuration = ref(props.timeout === undefined ? 5000 : props.timeout);
  const paginationEnabled = ref(props.pagination === undefined ? true : props.pagination)
  const navEnabled = ref(props.navigation === undefined ? true : props.navigation)
  let intervalId = timeoutDuration.value;

  const nextSlide = () => {
    resetInterval();
    if (currentSlide.value === getSlideCount.value) {
      currentSlide.value = 1;
      return;
    }
    currentSlide.value += 1;
  }
  const prevSlide = () => {
    resetInterval();
    if (currentSlide.value === 1) {
       currentSlide.value = 1;
       return;
    }
    currentSlide.value -= 1;
  }

  const goToSlide = (index) => {
    resetInterval();
    currentSlide.value = index + 1;
  }
  const autoPlay = () => {
    intervalId = setInterval(() => nextSlide(), timeoutDuration.value)
  }
  const resetInterval = () => {
    console.log(intervalId);
    clearInterval(intervalId);
    autoPlayEnabled.value && autoPlay()
  }

  autoPlayEnabled.value && autoPlay();

  onMounted(() => getSlideCount.value = document.querySelectorAll('.slide').length);
</script>

<style lang="scss">

$color-primary: black;
$color-white: #fff;
$shadow-light: 0 1px 3px 0 rgba(0, 0, 0, 0.1), 0 1px 2px 0 rgba(0, 0, 0, 0.06);

.navigate {
  padding: 0 16px;
  height: 100%;
  width: 100%;
  position: absolute;
  display: flex;
  justify-content: center;
  align-items: center;

  .toggle-page {
    display: flex;
    flex: 1;
  }

  .right {
    justify-content: flex-end;
  }

  i {
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 50%;
    width: 40px;
    height: 40px;
    background-color: $color-primary;
    color: $color-white;
  }
}

.pagination {
  position: absolute;
  bottom: 24px;
  width: 100%;
  display: flex;
  gap: 16px;
  justify-content: center;
  align-items: center;

  span {
    cursor: pointer;
    width: 20px;
    height: 20px;
    border-radius: 50%;
    background-color: $color-white;
    box-shadow: $shadow-light;
  }

  .active {
    background-color: $color-primary;
  }
}
</style>