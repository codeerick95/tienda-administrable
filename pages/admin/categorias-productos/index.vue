<template>
  <div class="card admin-categories">
    <div class="card-body">
      <div class="d-flex justify-content-between align-items-center">
        <h4 class="header-title">Lista de categorías</h4>

        <nuxt-link to="/admin/categorias-productos/crear" class="btn btn-sm btn-primary">
          Crear categoría
        </nuxt-link>
      </div>

      <div class="container mt-3">
        <div class="row">
          <div class="col-md-3" v-for="c in categories" :key="c.id">
            <div class="card bg-light text-center">
              <img class="card-img-top img-fluid admin-categories__image" :src="c.imagen.url" alt="Card image cap">
              <div class="card-body">
                <h2 class="card-title font-size-16 mt-0 font-weight-bold">{{ c.nombre }}</h2>

                <p class="card-text">
                  {{ c.descripcion }}
                </p>

                <section>
                  <nuxt-link :to="{name: 'admin-categorias-productos-editar-slug', params: {slug: c.slug}}" class="card-link">Editar</nuxt-link>

                  <a href="#" class="card-link text-danger" @click.prevent="showModalDelete(c)">Eliminar</a>
                </section>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <b-modal id="modal-categories-delete" hide-header hide-footer centered>
      <div class="text-center">
        <h2 class="my-4 lead font-weight-bold">¿Está seguro de eliminar la categoría?</h2>
        <span class="lead text-dark">{{ objectSelected.nombre }}</span>

        <section class="text-right mt-3">
          <a href="" class="text-danger mr-3" @click.prevent="closeModalDelete()">Cancelar</a>

          <button class="btn btn-primary" :disabled="loading ? true : false" @click.prevent="deleteColor()">{{ loading ? 'Eliminando' : 'Eliminar' }}</button>
        </section>
      </div>
    </b-modal>
  </div>
</template>

<script>
  // Queries
  import GetCategorias from '@/apollo/queries/categorias/GetCategorias'

  // Mutations
  import DeleteCategoria from '@/apollo/mutations/categorias-productos/DeleteCategoria'

  export default {
    middleware: 'authDashboard',
    layout: 'admin',
    data() {
      return {
        categories: [],
        objectSelected: {},
        loading: false
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
      },
      showModalDelete(category) {
        this.objectSelected = category

        this.$bvModal.show('modal-categories-delete')
      },
      closeModalDelete() {
        this.objectSelected = {}

        this.$bvModal.hide('modal-categories-delete')
      },
      deleteColor() {
        this.loading = true

        let input = {
          id: this.objectSelected.id
        }

        this.$apollo.mutate({
          mutation: DeleteCategoria,
          variables: {
            input
          }
        })
          .then(() => {
            this.getCategories()

            this.loading = false

            this.closeModalDelete()

            this.$toast.success('Categoría eliminada.', {
              duration : 3000
            })
          })
          .catch(() => this.loading = false)
      }
    },
    computed: {
    }
  }
</script>

<style lang="scss">
.admin-categories {
  &__image {
    height: 150px;
  }
}
</style>
