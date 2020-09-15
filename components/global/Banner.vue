<template>
  <section class="banner position-relative" v-if="loadCarousel">

    <!-- Mobile -->
    <swiper class="swiper d-md-none" :options="swiperOption" v-if="loadCarousel">
      <swiper-slide v-for="slide in sliders" :key="slide.id" class="h-100">
        <a href="" class="d-inline-block swiper-slide-mobile" @click.prevent="to(slide)">
          <img :src="slide.banner.url" alt="" class="w-100 h-100 slide-image-mobile">
        </a>
      </swiper-slide>
      <div class="swiper-pagination" slot="pagination"></div>
    </swiper>

    <!-- Desktop -->
    <swiper class="swiper d-none d-md-block" :options="swiperOption">
      <swiper-slide v-for="slide in sliders" :key="slide.id">
        <a href="" @click.prevent="to(slide)">
          <img :src="slide.banner.url" :alt="slide.nombre" class="banner__image w-100 h-100">
        </a>
      </swiper-slide>
      <div class="swiper-pagination" slot="pagination"></div>
    </swiper>
  </section>
</template>

<script>
  import GetSliders from '@/apollo/queries/sliders/GetSliders'

  export default {
    data() {
      return {
        sliders: [],
        swiperOption: {
          direction: 'vertical',
          pagination: {
            el: '.swiper-pagination',
            clickable: true
          },
          autoplay: {
            delay: 5000,
            disableOnInteraction: false
          }
        },
        loadCarousel: false
      }
    },
    mounted() {
      this.getSliders()

      setTimeout(() => {
        this.loadCarousel = true
      }, 1000)
    },
    methods: {
      getSliders() {
        this.$apollo.query({
          query: GetSliders,
          fetchPolicy: 'no-cache',
        })
        .then(res => this.sliders = res.data.GetSliders)
      },
      to(slide) {
        if(slide.url != '' || slide.url != null) {
          window.open(`https://${slide.url}`)
        }
      }
    }
  }
</script>

<style lang="scss">
.banner {
  height: 25vh;

  @media (min-width: 1200px) {
    height: 45vh;
  }

  .swiper-slide-mobile {
    height: 25vh;

    background-size: cover;
  }

  @include shadow;

  .swiper-container {
    height: 25vh;

    border-radius: .2rem;

    @media (min-width: 1200px) {
      height: 45vh;
    }

    /* &::before {
      content: '';

      background: rgb(36,50,56);
      background: linear-gradient(90deg, rgba(36,50,56,0.29175420168067223) 47%, rgba(36,50,56,0.7231267507002801) 100%);

      width: 100%;
      height: 100%;

      position: absolute;
      top: 0;
      left: 0;
      z-index: 10;
    } */
  }

  .swiper-pagination.swiper-pagination-clickable.swiper-pagination-bullets {
    position: absolute;
    right: 1.5rem;
    z-index: 100 !important;
  }

  .swiper-pagination-bullet {
    background-color: transparent;

    width: 15px;
    height: 15px;

    border: 1px solid $danger;

    opacity: 1;

    transform: scale(1);

    transition: transform .5s;

    &:hover {
      transform: scale(.9);
    }

    &-active {
      background-color: $danger;
    }
  }

  &__image {
    border-radius: .2rem;

    filter: grayscale(.3);

    cursor: pointer;
  }

  .slide-image-mobile {
    object-position: center;
  }
}
</style>
