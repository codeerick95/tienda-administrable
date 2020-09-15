<template>
  <section class="menu-desktop bg-white">
    <div class="menu-desktop__logo-container d-flex justify-content-center align-items-center text-center">
      <nuxt-link to="/">
        <img src="/logo.webp" alt="Logo Kira Sports" class="menu-desktop__logo">
      </nuxt-link>
    </div>

    <div class="text-center pb-2 pt-0 d-flex justify-content-center align-items-center">
      <a href="" class="social-icon mr-2 text-primary">
        <i class="fab fa-facebook"></i>
      </a>

      <a href="" class="social-icon mr-2 text-danger">
        <i class="fab fa-instagram"></i>
      </a>

      <a href="" class="social-icon mr-2 text-success">
        <i class="fab fa-whatsapp"></i>
      </a>
    </div>

    <nav class="menu-desktop__nav h-100">
      <b-card
        border-variant="danger"
        header-bg-variant="danger"
        header-text-variant="white"
        body-bg-variant="light"
        class="h-100"
      >
        <template v-slot:header>
          <div class="d-flex justify-content-center align-items-center">
            <h2 class="menu-desktop__title my-0 text-white">Comprar por categoría</h2>

            <span class="icon ml-2 pt-1">
              <i class="fas fa-tags"></i>
            </span>
          </div>
        </template>

        <section class="menu-desktop__nav-container d-flex flex-column h-100">
          <nuxt-link :to="{name: 'categorias-slug', params: {slug: category.slug}}" class="menu-desktop__link d-flex justify-content-between align-items-center" v-for="(category, index) in categories" :key="index">
            <div class="d-flex align-items-center">
              <div class="menu-desktop__image-container d-flex justify-content-center align-items-center">
                <img :src="category.imagen.url" alt="" class="menu-desktop__link-image">
              </div>

              <span class="d-inline-block ml-2">{{ category.nombre }}</span>
            </div>

            <span class="menu-desktop__icon">
              <i class="fas fa-chevron-circle-right"></i>
            </span>
          </nuxt-link>
        </section>
      </b-card>
    </nav>
  </section>
</template>

<script>
import { appConfig } from "@/env";

import GetCategorias from '@/apollo/queries/categorias/GetCategorias'

export default {
  data() {
    return {
      logo: appConfig.logo,
      facebook: appConfig.redesSociales.facebook,
      instagram: appConfig.redesSociales.instagram,
      whatsapp: appConfig.redesSociales.whatsapp,
      categories: []
    }
  },
  mounted() {
    this.getCategories()
  },
  methods: {
    getCategories() {
      let estado = "1"

      this.$apollo.query({
        query: GetCategorias,
        fetchPolicy: 'no-cache',
        variables: {
          estado
        }
      })
      .then(res => {
        this.categories = res.data.GetCategoria
      })
    }
  }
}
</script>

<style lang="scss">
.menu-desktop {

  @media (min-width: 1200px) {
    height: 100vh;
  }

  &__logo-container {
    height: 10vw;
  }

  &__logo {
    max-width: 60%;
  }

  &__title {
    font-size: 1em;
  }

  .card {
    border: none;

    @include shadow;
  }

  .card-header {
    padding: .4rem;
    border: none;
    border-radius: 0;
  }

  .card-body {
    background-color: rgba($light, .3);

    width: 23vw;
    /* height: 85vh; */

    /* @media (min-width: 1200px) {
      height: 65vh;
    } */

    padding: 0;

    /* position: fixed;
    bottom: 0; */

    overflow-y: scroll;

    &::-webkit-scrollbar {
      width: 8px;     /* Tamaño del scroll en vertical */
      height: 8px;    /* Tamaño del scroll en horizontal */
    }

    &::-webkit-scrollbar-thumb {
      background: #ccc;
      border-radius: 4px;
    }

    &::-webkit-scrollbar-thumb:hover {
      background: #b3b3b3;
      box-shadow: 0 0 2px 1px rgba(0, 0, 0, 0.2);
    }

    &::-webkit-scrollbar-thumb:active {
      background-color: #999999;
    }

    &::-webkit-scrollbar-track {
      background: #e1e1e1;
      border-radius: 4px;
    }

    &::-webkit-scrollbar-track:hover,
    &::-webkit-scrollbar-track:active {
      background: #d4d4d4;
    }
  }

  &__link {
    color: $dark;
    font-size: 1em;
    font-weight: 500;

    padding: .5rem 1rem .5rem .3rem;
    border-bottom: 1px solid $light;

    &:hover {
      color: $dark;
      background-color: $light;

      text-decoration: none;

      .menu-desktop__icon {
        opacity: 1;
      }
    }
  }

  &__image-container {
    background-color: rgba($dark, .1);

    width: 40px;
    height: 40px;

    border-radius: 50%;
  }

  &__link-image {
    width: 60%;
  }

  &__icon {
    font-size: 1.2em;
    color: $danger;

    opacity: 0;

    transition: opacity .3s;
  }

  &__redes {
    max-width: 25px;

    transition: transform .7s;

    &:hover {
      transform: scale(.9);
    }
  }
}

.social-icon {
  font-size: 1.7em;

  display: inline-block;

  transition: transform .7s;

    &:hover {
      transform: scale(.9);
    }
}
</style>
