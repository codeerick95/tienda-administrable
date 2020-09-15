<template>
  <div class="card admin-categories">
    <div class="card-body">
      <div class="d-flex justify-content-between align-items-center">
        <h4 class="medium-text">Editar slider</h4>

        <nuxt-link to="/admin/sliders" class="btn btn-sm btn-primary">
          Lista de sliders
        </nuxt-link>
      </div>

      <form @submit.prevent="update()" class="container mt-5">
        <div class="row justify-content-center">
          <div class="col-md-8">
            <div class="form-group">
              <label for="nombre">Nombre de slider</label>
              <input type="text" id="nombre" class="form-control" v-model="name">
            </div>

            <div class="form-group">
              <label for="url">Url a redirigir</label>
              <input type="url" id="number" class="form-control" v-model="url">
            </div>

            <div class="form-group text-right">
              <input type="submit" class="btn btn-primary" :disabled="!validate || loading ? true : false" :value="loading ? 'Actualizando...' : 'Actualizar'">
            </div>
          </div>

          <div class="col-md-4">
            <section class="border py-3 px-2">
              <span class="text-muted">Imagen destacada</span>
              <br>
              <button type="button" class="btn btn-sm btn-outline-warning" @click="showModalImages('destacada')">Seleccionar imagen</button>

              <img :src="image.url" alt="Imagen previa" class="img-fluid preview-image" v-if="image.id">
            </section>
          </div>
        </div>
      </form>
    </div>

    <gallery-modal @select="setImage($event)" @closeModal="closeModalImages()"></gallery-modal>
  </div>
</template>

<script>
  import UpdateSlider from '@/apollo/mutations/sliders/UpdateSlider'

  import GalleryModal from "@/components/global/images/GalleryModal";

  // Queries
  import GetSliderID from '@/apollo/queries/sliders/GetSliderID'

  export default {
    middleware: 'authDashboard',
    layout: 'admin',
    data() {
      return {
        id: this.$route.params.id,
        name: '',
        url: '',
        image: {},
        loading: false
      }
    },
    mounted() {
      this.getSlider()
    },
    components: {
      GalleryModal
    },
    methods: {
      getSlider() {
        let id = this.id

        this.$apollo.query({
          query: GetSliderID,
          variables: {
            id
          }
        })
        .then(response => {
          let info = response.data.GetSliderID
          this.name = info.nombre
          this.url = info.url
          this.image = info.banner
        })
      },
      showModalImages(type) {
        this.typeImage = type

        this.$bvModal.show('modal-images')
      },
      closeModalImages() {
        this.$bvModal.hide('modal-images')
      },
      setImage(image) {
        this.image = image

        this.closeModalImages()
      },
      update() {
        this.loading = true

        let input = {
          id: parseInt(this.id),
          nombre: this.name,
          url: this.url,
          banner: this.image.id
        }

        this.$apollo.mutate({
          mutation: UpdateSlider,
          variables: {
            input
          }
        })
          .then(() => {
            this.$router.push('/admin/sliders')

            this.$toast.success('Slider actualizado.', {
              duration : 3000
            })
          })
          .catch(() => {
            this.loading = false
          })
      }
    },
    computed: {
      validate: function() {
        let status = false

        if(this.name && this.image.id) {
          status = true
        }

        return status
      }
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
