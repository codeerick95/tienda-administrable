<template>
<nav class="nav-mobile bg-dark bg-info">
  <section class="nav-mobile__menu h-100 d-flex justify-content-center align-items-center">
    <div
      class="nav-mobile__link-container d-flex flex-column text-center"
      :class="item.tag == activeItem ? 'text-danger' : 'text-white'"
      v-for="(item, index) in items"
      :key="index"
      @click="to(item)"
    >
      <span class="icon">
        <i :class="item.icon"></i>
      </span>
      <span class="nav-mobile__link-text">{{ item.name}}</span>
    </div>
  </section>
</nav>
</template>

<script>
import {appConfig} from "@/env";

import { mapState } from 'vuex'

export default {
  data() {
    return {
      items: [
        {
          status: false,
          name: 'Inicio',
          icon: 'fas fa-home',
          tag: 'inicio',
          route: '/'
        },
        {
          status: false,
          name: 'Categorías',
          icon: 'fas fa-tags',
          tag: 'categorias'
        },
        {
          status: false,
          name: 'Carrito',
          icon: 'fas fa-user-circle',
          tag: 'carrito'
        },
        {
          status: false,
          name: 'Cuenta',
          icon: 'fas fa-user-circle',
          tag: 'cuenta',
          route: '/mi-cuenta'
        }
      ],
      activeItem: 'inicio',
      loading: false
    }
  },
  methods: {
    to(item) {
      if(item.tag == 'categorias') {
        this.$store.commit('setShowCategoriesMobile', true)
      } else {
        // Cierra modal de categorías
        this.$store.commit('setShowCategoriesMobile', false)

        if(item.tag == 'carrito') {
          this.$store.commit('setModalCarrito', true)
        }

        if(item.tag == 'inicio') {
          this.$router.push(item.route)
        }

        if(item.tag == 'cuenta') {

          if(this.currentUser) {
            // Redirigir a cuenta
            if(this.userData.typeUser == 1) {
              this.$router.push('/admin/productos')
            } else {
              this.$router.push('/mi-cuenta')
            }
          } else {
            this.$bvModal.show('modal-auth')
          }
        }
      }

      // Asignar item activo
      this.activeItem = item.tag
    }
  },
  computed: {
    ...mapState(['showCategoriesMobile', 'modalCarrito']),
    currentUser: function () {
      return !!this.$apolloHelpers.getToken()
    },
    userData: function () {
      return this.$cookies.get(appConfig.userData)
    }
  }
}
</script>

<style lang="scss">
.nav-mobile {
  max-width: 100%;
  height: 10vh;

  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 100;

  &__menu {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-column-gap: .1rem;

    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
  }

  &__link-container {
    margin: 0 .5rem;
    padding: .5rem;
  }

  &__link-text {
    font-size: .9em;
  }
}
</style>
