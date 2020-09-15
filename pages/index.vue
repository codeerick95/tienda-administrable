<template>
  <div class="products mt-4">
    <div class="container">
      <div class="row pt-0 mb-3">
        <div class="col-md-12 pt-0">
          <sub-header title="Nuestros productos"></sub-header>
        </div>
      </div>

      <div class="row">
        <template>
          <div class="col-6 col-md-3 mb-lg-3 mb-lg-5 mb-lg-4 px-0 px-lg-2" v-for="product in products" :key="product.id">
            <product :product="product"></product>
          </div>
        </template>

        <div class="col-md-12 py-0" v-if="loading">
          <loading></loading>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { appConfig } from '@/env'
import { mapState } from 'vuex'

// Queries
import GetProductos from '@/apollo/queries/products/GetProductos'

// Components
import Product from "@/components/products/Product";
import Loading from "@/components/global/Loading";
import SubHeader from "@/components/global/SubHeader";

export default {
  data() {
    return {
      products: [],
      number_paginate: 12,
      page: 1,
      nroTotalItems: 0,
      loading: false
    }
  },
  mounted() {
    this.getProducts()
      .then(products => {
        this.products = products
      })

    setTimeout(() => {
      this.infiniteScroll()
    }, 1000)
  },
  head () {
    let title = appConfig.openGraph.titulo,
      description = appConfig.openGraph.descripcion,
      logo = appConfig.openGraph.logo,
      url = appConfig.openGraph.urlWeb,
      whatsappImg = appConfig.openGraph.logoWhatsapp,
      keywords = appConfig.openGraph.keywords

    return {
      title,
      meta: [

        {name: 'keywords', content: keywords},

        // Facebook OpenGraph
        {property: 'og:url', content: url},
        {property: 'og:title', content: title},
        {property: 'og:site_name', content: title},
        {property: 'og:type', content: 'website'},
        {property: 'og:image', content: logo},
        {property: 'og:description', content: description},

        // Whatsapp OpenGraph
        {property: 'og:url', content: url},
        {property: 'og:title', content: title},
        {property: 'og:site_name', content: title},
        {property: 'og:type', content: 'website'},
        {property: 'og:image', content: whatsappImg},
        {property: 'og:description', content: description},

	// Twitter Card
        {name: 'twitter:card', content: 'summary'},
        {name: 'twitter:title', content: title},
        {name: 'twitter:description', content: description},
        {name: 'twitter:image', content: logo}
      ]
    }
  },
  components: {
    Product,
    Loading,
    SubHeader
  },
  methods: {
    getProducts() {
      return new Promise(resolve => {
        this.loading = true

        let number_paginate = this.number_paginate,
          page = this.page,
          estado = "1"

        this.$apollo.query({
          query: GetProductos,
          variables: {
            number_paginate,
            page,
            estado
          }
        })
          .then(res => {
            this.nroTotalItems = res.data.GetProductos.NroItems

            this.loading = false

            resolve(res.data.GetProductos.data)
          })
      })
    },
    infiniteScroll() {
      window.onscroll = () => {
        // True || False
        let bottomOfWindow =
          document.documentElement.scrollTop + window.innerHeight ===
          document.documentElement.offsetHeight;

        if (bottomOfWindow) {
          // Preguntamos si la página actual es menor al total de páginas la Api
          if (this.page < (this.nroTotalItems / this.number_paginate)) {
            // Si es así, aumentamos la página actual en 1
            // importante asignarle el signo < ya que si es igual o mayor, entonces
            // se le sumará 1 y la api devolverá un error
            this.page = this.page + 1;

            // Y hacemos el pedido de los nuevos datos
            this.getProducts()
              .then(products => {
                products.forEach(item => this.products.push(item))
              })
          }
        }
      }
    }
  }
}
</script>

<style lang="scss">
.products {
  &__subtitle {
    @include subtitle;

    &::first-letter {
      text-transform: uppercase;
    }
  }
}
</style>
