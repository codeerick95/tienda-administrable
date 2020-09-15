<template>
  <section class="admin-sidebar position-relative">
    <div class="logo d-flex justify-content-center">
      <img src="/logo.webp" alt="" class="admin-sidebar__logo">
    </div>

    <nav class="admin-nav px-3">
      <div class="w-100 position-relative overflow-hidden" v-for="(item, index) in items" :key="index">
        <a
          href=""
          class="admin-nav__link d-flex justify-content-between align-items-center w-100 p-2"
          :class="{'text-white' : item.status }"
          @click.prevent="clicked(item, index)"
        >

          <div>
          <span class="admin-nav__icon mr-2 mt-1">
            <i :class="item.icon"></i>
          </span>

            {{ item.text }}
          </div>

          <span class="admin-nav__icon" v-if="item.subItems">
          <i class="fas fa-angle-down" :class="{'fas fa-sort-up' : item.subItemsStatus}"></i>
        </span>
        </a>

        <section
          class="admin-nav__link-items pl-4"
          :class="{'admin-nav__link-items--active' : item.subItemsStatus}"
          v-if="item.subItems">

          <nuxt-link
            :to="subitem.route"
            class="admin-nav__link d-flex w-100 p-2"
            v-for="(subitem, index) in item.subItems"
            :key="index"
          >

          <span class="admin-nav__icon mr-2">
            <i class="fas fa-th-large"></i>
          </span>

            {{ subitem.text }}
          </nuxt-link>

        </section>
      </div>

      <button type="button" class="btn btn-block btn-danger" :disabled="loading" @click="logout()">{{ loading ? 'Saliendo...' : 'Cerrar sesión' }}</button>
    </nav>
  </section>
</template>

<script>
  import { appConfig } from '@/env'
  
  export default {
    data() {
      return {
        items: [
          {
            status: true,
            text: 'Visitar tienda',
            icon: 'fas fa-th-large',
            route: '/'
          },
          {
            status: false,
            text: 'Pedidos',
            icon: 'fas fa-pallet',
            route: '/admin/pedidos'
          },
          {
            status: false,
            text: 'Productos',
            icon: 'fas fa-th-large',
            subItemsStatus: true,
            subItems: [
              {
                status: false,
                text: 'Ver todos',
                icon: 'fas fa-th-large',
                route: '/admin/productos'
              },
              {
                status: false,
                text: 'Crear',
                icon: 'fas fa-th-large',
                route: '/admin/productos/crear'
              }
            ]
          },
          {
            status: false,
            text: 'Categorías',
            icon: 'fas fa-tags',
            subItemsStatus: false,
            subItems: [
              {
                status: false,
                text: 'Listar',
                icon: 'fas fa-th-large',
                route: '/admin/categorias-productos'
              },
              {
                status: false,
                text: 'Crear',
                icon: 'fas fa-th-large',
                route: '/admin/categorias-productos/crear'
              },
            ]
          },
          {
            status: false,
            text: 'Bancos',
            icon: 'far fa-building',
            subItemsStatus: false,
            subItems: [
              {
                status: false,
                text: 'Listar',
                icon: 'fas fa-th-large',
                route: '/admin/bancos'
              },
              {
                status: false,
                text: 'Crear',
                icon: 'fas fa-th-large',
                route: '/admin/bancos/crear'
              },
            ]
          },
          {
            status: false,
            text: 'Efectivo móvil',
            icon: 'fas fa-th-large',
            subItemsStatus: false,
            subItems: [
              {
                status: false,
                text: 'Listar',
                icon: 'fas fa-th-large',
                route: '/admin/efectivo-movil'
              },
              {
                status: false,
                text: 'Crear',
                icon: 'fas fa-th-large',
                route: '/admin/efectivo-movil/crear'
              },
            ]
          },
          {
            status: false,
            text: 'Sliders',
            icon: 'fas fa-th-large',
            subItemsStatus: false,
            subItems: [
              {
                status: false,
                text: 'Listar',
                icon: 'fas fa-th-large',
                route: '/admin/sliders'
              },
              {
                status: false,
                text: 'Crear',
                icon: 'fas fa-th-large',
                route: '/admin/sliders/crear'
              }
            ]
          },
          {
            status: false,
            text: 'Recursos',
            icon: 'fas fa-th-large',
            subItemsStatus: false,
            subItems: [
              {
                status: false,
                text: 'Imágenes',
                icon: 'fas fa-th-large',
                route: '/admin/recursos/imagenes'
              }
            ]
          }
        ],
        loading: false
      }
    },
    methods: {
      clicked(item, index) {
        this.items.forEach(i => {
          i.status = false
          i.subItemsStatus = false
        })

        this.items[index].status = true

        if(this.items[index].subItems) {
          this.items[index].subItemsStatus = true
        }

        if(item.route) {
          this.$router.push(item.route)
        }
      },
      logout() {
          this.loading = true

          setTimeout(() => {
              this.$apolloHelpers.onLogout()
              .then(() => {
                  this.$cookies.remove(appConfig.userData)

                  if(this.$route.path === '/') {
                      this.$store.commit('reloadPage')
                  } else {
                      this.$router.push('/')
                  }

                  this.loading = false
              })
          }, 1000)
      }
    }
  }
</script>

<style lang="scss">
  .admin-sidebar {
    &__logo {
      max-width: 50%;
    }

    &__logout {
      position: absolute;
      bottom: 0;
      left: 0;
    }
  }

  .admin-nav {
    &__link,
    &__icon {
      font-size: .9em;
      color: rgba(white, .7);

      display: inline-block;

      transition: color .3s, background-color .2s;

      &:hover {
        color: white;

        text-decoration: none;

        .admin-nav__icon {
          color: white;
        }
      }

      &:active{
        background-color: $dark;
      }

      &-items {
        height: 0;
        opacity: 0;

        transition: opacity 1s;

        &--active {
          height: auto;
          opacity: 1;
        }
      }
    }

    &__link--items {
      position: relative;
    }
  }
</style>
