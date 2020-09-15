<template>
  <div class="products mt-4">
    <div class="container">
      <div class="row pt-0 mb-3">
        <div class="col-md-12 pt-0">
          <sub-header :title="notSlug"></sub-header>
        </div>
      </div>

      <div class="row" v-if="products.length >= 1">
        <div class="col-6 col-md-3 mb-lg-3 mb-lg-5 mb-lg-4 px-0 px-lg-2" v-for="(product, index) in products" :key="index">
          <product :product="product"></product>
        </div>

        <div class="col-md-12 py-0" v-if="loading">
          <loading></loading>
        </div>
      </div>

      <div class="row" v-else>
        <div class="col-md-12">
          <p class="lead">No se encontraron productos.</p>
        </div>
      </div>

    </div>
  </div>
</template>

<script>
  import { appConfig } from '@/env'

  import GetProductoCategoria from '@/apollo/queries/categorias/GetProductoCategoria'
  import GetCategoriaSlug from '@/apollo/queries/categorias/GetCategoriaSlug'

  // Components
  import Product from "@/components/products/Product";
  import Loading from "@/components/global/Loading";

  export default {
    data() {
      return {
        slug: this.$route.params.slug,
        products: [],
        number_paginate: 8,
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
    apollo: {
      GetCategoriaSlug: {
        prefetch: true,
        query: GetCategoriaSlug,
        fetchPolicy: 'no-cache',
        variables() {
          return {
            slug: this.slug
          }
        }
      }
    },
    head () {
      // Se utiliza https://vue-meta.nuxtjs.org/

      let slug = this.slug

      let title = this.notSlug,
        description = appConfig.openGraph.descripcion,
        logo = appConfig.openGraph.logo,
        url = appConfig.openGraph.urlWeb,
        whatsappImg = appConfig.openGraph.logo,
        keywords = appConfig.openGraph.keywords

      if(this.category) {
        description = this.category.descripcion
        title = this.category.nombre // Graph
        logo = this.category.open_graph.url
        keywords = this.category.keywords
      }

      return {
        title,
        meta: [

          {name: 'keywords', content: keywords},

          // Twitter Card
          {name: 'twitter:card', content: 'summary'},
          {name: 'twitter:title', content: title},
          {name: 'twitter:description', content: description},
          {name: 'twitter:image', content: logo},

          // Facebook OpenGraph
          {property: 'og:url', content: `${url}/categorias/${slug}`},
          {property: 'og:title', content: title},
          {property: 'og:site_name', content: title},
          {property: 'og:type', content: 'website'},
          {property: 'og:image', content: logo},
          {property: 'og:description', content: description},

          // Whatsapp OpenGraph
          {property: 'og:url', content: `${url}/categorias/${slug}`},
          {property: 'og:title', content: title},
          {property: 'og:site_name', content: title},
          {property: 'og:type', content: 'website'},
          {property: 'og:image', content: whatsappImg},
          {property: 'og:description', content: description}
        ]
      }
    },
    components: {
      Product,
      Loading
    },
    methods: {
      getProducts() {
        return new Promise(resolve => {
          this.loading = true

          let slug = this.slug,
            number_paginate = this.number_paginate,
            page = this.page,
            estado = "1"

          this.$apollo.query({
            query: GetProductoCategoria,
            variables: {
              slug,
              number_paginate,
              page,
              estado
            }
          })
          .then(res => {
            const productos = this.products = res.data.GetProductoCategoria.data

            this.nroTotalItems = res.data.GetProductoCategoria.NroItems

            this.loading = false

            resolve(productos)
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
    },
    computed: {
      category: function () {
        if(this.GetCategoriaSlug) {
          return this.GetCategoriaSlug
        }
      },
      notSlug: function () {
        return this.slug.replace('-', ' ')
      }
    }
  }
</script>

<style lang="scss">
.categoria-slug__subtitle {
  display: inline-block;
  border-bottom: 2px solid $danger;
}
</style>
