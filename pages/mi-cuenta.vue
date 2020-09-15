<template>
  <div class="container mt-4">
    <div class="row">
      <div class="col-md-12">
        <sub-header :title="`Bienvenido ${user.name}`" v-if="user"></sub-header>

        <section class="mt-3">
          <div class="card">
            <div class="card-body">
              <h3 class="lead font-weight-bold">Mis pedidos</h3>

              <div class="table-responsive" v-if="orders.length >= 1">
                <table class="table mb-0 text-center">
                  <thead>
                  <tr>
                    <th>#</th>
                    <th>Estado</th>
                    <th>Fecha de pedido</th>
                    <th>Tipo de envío</th>
                    <th>Monto total</th>
                    <th>Acciones</th>
                  </tr>
                  </thead>
                  <tbody>
                  <tr v-for="order in orders" :key="order.id">
                    <th scope="row">{{ order.id }}</th>

                    <td>
                      <span class="font-weight-bold">{{ setState(order.EstadoPedido) }}</span>
                    </td>

                    <td>
                      <span>{{ $moment(order.fechaPedido).format('DD-MM-YYYY') }}</span>
                    </td>

                    <td>
                      <span>{{ setTipoEnvio(order.TipoEnvio) }}</span>
                    </td>

                    <td>
                      <span>S/{{ parseFloat(order.precioTotal) }}</span>
                    </td>

                    <td>
                      <nuxt-link :to="{name: 'pedidos-id', params: {id: order.id}}" class="btn btn-sm btn-info">Detalles</nuxt-link>
                    </td>
                  </tr>
                  </tbody>
                </table>
              </div>

              <section class="mt-3" v-else>
                <div class="d-flex flex-column flex-md-row align-items-center">
                  <p class="my-0">Aún no tiene pedidos registrados.</p>
                  <nuxt-link to="/" class="btn btn-sm btn-primary mt-2 mt-md-0 ml-3">Visitar tienda</nuxt-link>
                </div>
              </section>
            </div>
          </div>
        </section>
      </div>
    </div>
  </div>
</template>

<script>
  import GetPedidoUser from '@/apollo/queries/pedidos/GetPedidoUser'
  import {appConfig} from "../env";

export default {
  middleware: 'validateUser2',
  data() {
    return {
      orders: [],
      number_paginate: 25,
      page: 1,
      loading: false
    }
  },
  mounted() {
    if(this.user.typeUser == 1) {
      this.$router.push('/')
    } else {
      this.getOrders()
    }
  },
  methods: {
    getOrders() {
      let number_paginate = this.number_paginate,
        page = this.page

      this.$apollo.query({
        query: GetPedidoUser,
        fetchPolicy: 'no-cache',
        variables: {
          number_paginate,
          page
        }
      })
      .then(r => {
        this.orders = r.data.GetPedidoUser
      })
    },
    setState(value) {
      let status

      if(value == 1) {
        status = 'En espera'
      } else if(value == 2) {
        status = 'Validado'
      }

      return status
    },
    setTipoEnvio(value) {
      let status

      if(value == 1) {
        status = 'Envío Express'
      } else if(value == 2) {
        status = 'Encomienda (Provincias)'
      } else {
        status = 'Envío gratuito'
      }

      return status
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
  },
  computed: {
    user: function() {
      if(this.$cookies.get(appConfig.userData)) {
        let user = this.$cookies.get(appConfig.userData)

        return user
      }
    }
  }
}
</script>

<style scoped>

</style>
