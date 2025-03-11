<script setup>
import { ref, computed } from "vue";
import { Swiper, SwiperSlide } from "swiper/vue";
import "swiper/css";
import "swiper/css/navigation";
import "swiper/css/pagination";
import { Navigation, Pagination } from "swiper/modules";

const albums = ref([
  { id: 1, title: "HOP", year: 2024, image: "hop.jpg" },
  { id: 2, title: "ATE", year: 2024, image: "ate.jpg" },
  { id: 3, title: "ROCK-STAR", year: 2023, image: "rockstar.jpg" },
  { id: 4, title: "5-STAR", year: 2023, image: "5star.jpg" },
]);
const duplicatedAlbums = computed(() => [...albums.value, ...albums.value]);

const swiperInstance = ref(null);
const currentIndex = ref(0);

const onSwiper = (swiper) => {
  swiperInstance.value = swiper;
};

const nextSlide = () => {
  if (swiperInstance.value) {
    /**
     * Updates the value of `currentIndex` based on the current slide of the swiper instance.
     * @param {Object} swiper - The swiper instance.
     */
    swiperInstance.value.slideNext();
  }
};

const updateCurrentIndex = (swiper) => {
  if (swiper.realIndex < 4) {
    currentIndex.value = swiper.realIndex;
  } else{
    currentIndex.value = swiper.realIndex - 4
  }
};

const formattedIndex = computed(() => {
  return String(currentIndex.value + 1).padStart(2, "0");
});
</script>

<template>
  <section class="discography" id="discography">
    <div class="container">
      <h1 class="discography__title title">DISCOGRAPHY</h1>
      <swiper
        @swiper="onSwiper"
        @slideChange="updateCurrentIndex"
        :slidesPerView="4"
        :spaceBetween="20"
        :centeredSlides="false"
        :slidesPerGroup="1"
        :loop="true"
        :navigation="false"
        class="discography__swiper"
      >
        <swiper-slide
          v-for="(album, index) in duplicatedAlbums"
          :key="album.id"
          class="discography__swiper-slide"
          :class="{ 'active-slide': index === currentIndex }"
        >
          <img
            class="discography__swiper-img"
            :src="album.image"
            :alt="album.title"
          />
          <div class="discography__swiper-box">
            <h3 class="discography__swiper-title">{{ album.title }}</h3>
            <p class="discography__swiper-year">{{ album.year }}</p>
          </div>
        </swiper-slide>
        <div class="discography__controls">
          <button class="discography__next-btn" @click="nextSlide">NEXT</button>
          <div class="discography__pagination">
            <hr />
            <span>{{ formattedIndex }}</span>
          </div>
        </div>
      </swiper>
    </div>
  </section>
</template>

<style src="./Discography.scss" scoped></style>
