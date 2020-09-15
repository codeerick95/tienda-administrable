<template>
  <div class="main-container container-fluid front">

    <!-- <section class="bg-dark text-right link-admin" v-if="userLogged && userData.typeUser == 1">
      <nuxt-link to="/admin/productos" class="btn btn-sm btn-primary rounded-0">Panel de administración</nuxt-link>
    </section> -->
    <!-- Load Facebook SDK for JavaScript -->
    <div id="fb-root"></div>

    <!-- Facebook messenger -->
    <div class="fb-customerchat"
      attribution=setup_tool
            page_id="105628061180244"
      theme_color="#009245"
      logged_in_greeting="Hola somos KiraSport ¿En que podemos ayudarte?"
      logged_out_greeting="Hola somos KiraSport ¿En que podemos ayudarte?">
    </div>
    
    <div class="row">
      <div class="col-md-3 first-column">
        <menu-desktop class="d-none d-lg-block"></menu-desktop>
      </div>
      <div class="col-lg-9 offset-lg-3 px-lg-0">

        <!-- No se muestra en la vista de un producto -->
        <banner v-if="$route.name != 'productos-slug' && $route.name != 'terminos-y-condiciones'"></banner>

        <Nuxt />

      </div>
    </div>

    <transition name="transition-categories" enter-active-class="animated fadeInDown" leave-active-class="animated fadeOut">
      <categories-mobile class="d-lg-none" v-if="showCategoriesMobile"></categories-mobile>
    </transition>

    <menu-mobile class="d-lg-none"></menu-mobile>

    <!-- <div class="cart-btn d-none d-lg-block bg-light shadow" :class="$route.name != 'productos-slug' ? 'cart-btn--medium' : 'cart-btn--top'" v-if="$route.name != 'terminos-y-condiciones' && $route.name != 'pedidos-id'">
      <div class="d-flex alig-items-center py-3 px-4" @click="mostrarCarrito()">
        <span class="icon d-flex justify-content-center align-items-center pr-3">
          <i class="fas fa-shopping-cart"></i>
        </span>

        <div class="cart-btn__info">
          <p class="text-muted my-0">
            <span class="font-weight-bold">{{ nroItemsCarrito }}</span>
            items
          </p>
          <p class="my-0 font-weight-bold">Mi carrito</p>
        </div>
      </div>
      <div class="text-center py-2 cart-btn__item">
        <a href="" class="text-white d-inline-block w-100" @click.prevent="toAccount()">Mi cuenta</a>
      </div>
      <div class="text-center py-2 cart-btn__item" v-if="userLogged">
        <a href="" class="text-white d-inline-block w-100" @click.prevent="logout()">
          {{ loading ? 'Saliendo...' : 'Salir' }}
          <i class="fas fa-sign-out-alt"></i>
        </a>
      </div>
    </div> -->

    <transition
      name="custom-classes-transition"
      enter-active-class="animated zoomInDown"
      leave-active-class="animated fadeOut"
    >
      <cart-modal v-if="modalCarrito" @closeModal="$store.commit('setModalCarrito', false)"></cart-modal>
    </transition>

    <modal-auth></modal-auth>
  </div>
</template>

<script>
import { appConfig } from "@/env";

import { mapState } from 'vuex'

import MenuDesktop from '@/components/global/MenuDesktop'
import MenuMobile from "@/components/global/MenuMobile";
import CategoriesMobile from "@/components/global/CategoriesMobile";
import Banner from '@/components/global/Banner'
import CartModal from "@/components/global/carrito/CartModal";
import ModalAuth from "@/components/global/auth/ModalAuth";

export default {
  data() {
    return {
      showModalCart: false,
      loading: false
    }
  },
  components: {
    MenuDesktop,
    CategoriesMobile,
    MenuMobile,
    Banner,
    ModalAuth,
    CartModal
  },
  mounted() {
    let cart = localStorage.getItem('kira_cart')

    if(cart) {
      // Realiza el conteo de productos en local storage
      let nroItems = JSON.parse(localStorage.getItem('kira_cart'))
      this.$store.commit('setNroItemsCarrito', nroItems.length)
    }

  },
  methods: {
    mostrarCarrito() {
      // Abrir modal de carrito
      this.$store.commit('setModalCarrito', true)
    },
    toAccount() {
      // Si no está logueado
      if(!this.userLogged) {

        this.$bvModal.show('modal-auth')

      } else {

        // Redirigir según el tipo de usuario
        if(this.userData.typeUser == 1) {
          this.$router.push('/admin/productos')
        } else if(this.userData.typeUser == 2){
          this.$router.push('/mi-cuenta')
        } else {
          this.$router.push('/')
        }

      }
    }
  },
  computed: {
    ...mapState(['modalCarrito', 'nroItemsCarrito', 'showCategoriesMobile']),
    userLogged: function () {
      return !!this.$apolloHelpers.getToken()
    },
    userData: function () {
      return this.$cookies.get(appConfig.userData)
    }
  }
}
</script>

<style lang="scss">
.main-container {
  padding-bottom: 5rem;
}

.first-column {
  position: fixed;
}

.cart-btn {
  border-bottom: 4px solid $danger;

  position: fixed;
  right: 1rem;
  z-index: 100;

  cursor: pointer;

  &--medium {
    bottom: 35vh;
  }

  &--top {
    bottom: 50vh;
  }

  &__item {
    background-color: rgba($danger, .9);

    border-top: 1px solid rgba($dark, .1);

    &:hover {
      background-color: $danger;
    }
  }
}
</style>
