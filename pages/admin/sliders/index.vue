<template>
  <div class="card admin-sliders">
    <div class="card-body">
      <div class="d-flex justify-content-between align-items-center">
        <h4 class="medium-text">Lista de sliders</h4>

        <nuxt-link to="/admin/sliders/crear" class="btn btn-sm btn-primary">
          Crear slider
        </nuxt-link>
      </div>

      <div class="container mt-3">
        <div class="row">
          <div class="col-md-4" v-for="item in sliders" :key="item.id">
            <div class="card">

              <div class="d-flex justify-content-center">
                <img class="img-fluid admin-sliders__image" :src="item.banner.url" alt="Card image cap">
              </div>

              <div class="card-body">
                <h2 class="medium-text mt-0">{{ item.nombre }}</h2>

                <p class="card-text">
                  <a :href="item.url">{{ item.url }}</a>
                </p>

                <section class="border-top pt-3 text-center">
                  <nuxt-link :to="{name: 'admin-sliders-editar-id', params: {id: item.id}}" class="card-link">Editar</nuxt-link>

                  <a href="#" class="card-link text-danger" @click.prevent="showModalDelete(item)">Eliminar</a>
                </section>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <b-modal id="modal-slider-delete" hide-header hide-footer centered>
      <div class="text-center">
        <h2 class="my-4 lead font-weight-bold">¿Está seguro de eliminar el slider?</h2>
        <span class="lead text-dark">{{ objectSelected.nombre }}</span>

        <section class="text-right mt-3">
          <a href="" class="text-danger mr-3" @click.prevent="closeModalDelete()">Cancelar</a>

          <button class="btn btn-primary" :disabled="loading ? true : false" @click.prevent="deleteSlider()">{{ loading ? 'Eliminando' : 'Eliminar' }}</button>
        </section>
      </div>
    </b-modal>
  </div>
</template>

<script>
  // Queries
  import GetSliders from '@/apollo/queries/sliders/GetSliders'

  // Mutations
  import DeleteSlider from '@/apollo/mutations/sliders/DeleteSlider'

  export default {
    middleware: 'authDashboard',
    layout: 'admin',
    data() {
      return {
        sliders: [],
        objectSelected: {},
        loading: false
      }
    },
    mounted() {
      this.getSliders()
    },
    methods: {
      getSliders() {
        this.$apollo.query({
          query: GetSliders,
          fetchPolicy: 'no-cache',
        })
          .then(res => {
            this.sliders = res.data.GetSliders
          })
      },
      showModalDelete(item) {
        this.objectSelected = item

        this.$bvModal.show('modal-slider-delete')
      },
      closeModalDelete() {
        this.objectSelected = {}

        this.$bvModal.hide('modal-slider-delete')
      },
      deleteSlider() {
        this.loading = true

        let input = {
          id: this.objectSelected.id
        }

        this.$apollo.mutate({
          mutation: DeleteSlider,
          variables: {
            input
          }
        })
          .then(() => {
            this.getSliders()

            this.loading = false

            this.closeModalDelete()

            this.$toast.success('Slider eliminado.', {
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
  .admin-sliders {
    &__image {
      max-width: 70%;
      height: 170px;
    }
  }
</style>
