<template>
  <div class="container error text-center mt-5">
    <img src="/logo.webp" alt="Logo KiraSports" class="error__logo">

    <section class="mt-3">
        <h1 class="error__title font-weight-bold" v-if="error.statusCode === 404">Página no encontrada</h1>
        <h1 class="error__title font-weight-bold" v-else>An error occurred</h1>
    </section>
    
    <div class="mt-3">
        <nuxt-link class="btn btn-danger rounded-0" to="/">Visitar tienda</nuxt-link>
    </div>
  </div>
</template>

<script>
  export default {
    props: ['error'],
    layout: 'default'
  }
</script>

<style lang="scss">
.error {
    &__title {
        font-size: 2em;
    }

    &__logo {
        max-width: 200px;
    }
}
</style>