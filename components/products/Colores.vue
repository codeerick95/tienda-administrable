<template>
<section class="product-colors">
    <div class="product-colors__container w-100">
        <div
        class="product-colors__color pointer d-flex justify-content-center align-items-center"
        v-for="(color, index) in colors"
        :key="index"
        :class="{'product-colors__color--active': color.status, 'product-colors__color--disabled': color.stok_real <= 0}"
        v-bind:style="{backgroundColor: color.Colores ? color.Colores.codColor : color.codColor}"
        @click="cambiarColor(index)"
        >
        </div>
    </div>
</section>
</template>

<script>
export default {
    props: ['colors'],
    methods: {
        cambiarColor(index) {
            this.$emit('cambiarColor', index)
        }
    }
}
</script>

<style lang="scss">
.product-colors {
    &__container {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(1rem, 1rem));
        grid-gap: .2rem;

        @media (min-width: 768px) {
          grid-template-columns: repeat(auto-fit, minmax(1rem, 1.3rem));
          grid-gap: .3rem;
        }
    }
}

.product-colors__color {
  width: 1rem;
  height: 1rem;


  border: 2px solid rgba($dark, .3);
  border-radius: 50%;

  position: relative;

  @media (min-width: 768px) {
    width: 1.3rem;
    height: 1.3rem;
  }

  transition: transform .5s;

  &:hover {
    transform: scale(1.05);
  }

    &--active {
      border: 2px solid $danger;
    }

    &--disabled {
      cursor: initial;

      opacity: .3;

      &::before {
        content: '';
        background-color: $danger;

        width: 110%;
        height: 3px;

        position: absolute;
        bottom: .7rem;
        left: 0;

        transform: rotate(-40deg);
      }
    }
}
</style>