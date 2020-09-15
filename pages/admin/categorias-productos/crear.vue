<template>
  <div class="card create-category">
    <div class="card-body">
      <div class="d-flex justify-content-between align-items-center">
        <h4 class="header-title">Crear categorías</h4>

        <nuxt-link to="/admin/categorias-productos" class="btn btn-sm btn-primary">
          Ver lista
        </nuxt-link>
      </div>

      <form @submit.prevent="createCategory()" class="create-category__form mt-3">
        <div class="form-row">

          <div class="col-md-8">
            <div class="form-group">
              <label for="name">Nombre de categoría</label>
              <input type="text" id="name" class="form-control" v-model="nombre">
            </div>

            <div class="form-group">
              <label for="description">Descripción</label>
              <textarea id="description" class="form-control" v-model="description"></textarea>
            </div>

            <div class="form-group">
              <label for="keywords">Palabras clave</label>
              <input type="text" id="keywords" class="form-control" v-model="keywords">
            </div>

            <div class="text-right">
              <input type="submit" :disabled="loading" :value="loading ? 'Creando...' : 'Crear'" class="btn btn-primary px-3">
            </div>
          </div>

          <div class="col-md-4 pl-lg-3">
            <section class="border py-3 px-2">
              <span class="d-block mb-2 font-weight-bold">Imagen destacada</span>
              <button type="button" class="btn btn-sm btn-outline-warning" @click="showModalImages('destacada')">Seleccionar imagen</button>

              <img :src="destacada.url" alt="Imagen previa" class="img-fluid preview-image" v-if="destacada.id">
            </section>

            <section class="border mt-3 py-3 px-2">
              <span class="d-block mb-2 font-weight-bold">Imagen para redes sociales</span>
              <button type="button" class="btn btn-sm btn-outline-warning" @click="showModalImages('openGraph')">Seleccionar imagen</button>

              <img :src="openGraph.url" alt="Imagen previa" class="img-fluid preview-image" v-if="openGraph.id">
            </section>
          </div>

        </div>

      </form>
    </div>

    <gallery-modal @select="setImage($event)" @closeModal="closeModalImages()"></gallery-modal>
  </div>
</template>

<script>
  import GalleryModal from "../../../components/global/images/GalleryModal";

  import CreateCategoria from '@/apollo/mutations/categorias-productos/CreateCategoria'

  export default {
    middleware: 'authDashboard',
    layout: 'admin',
    data() {
      return {
        nombre: '',
        description: '',
        imagen: '',
        keywords: '',
        loading: false,
        typeImage: 'destacada',
        destacada: {},
        openGraph: {}
      }
    },
    mounted() {
    },
    components: {
      GalleryModal
    },
    methods: {
      showModalImages(type) {
        this.typeImage = type

        this.$bvModal.show('modal-images')
      },
      closeModalImages() {
        this.$bvModal.hide('modal-images')
      },
      setImage(image) {
        if(this.typeImage == 'destacada') {
          this.destacada = image
        } else {
          this.openGraph = image
        }

        this.closeModalImages()
      },
      createCategory() {
        this.loading = true

        let input = {
          "nombre": this.nombre,
          "imagen": this.destacada.id,
          "open_graph": this.openGraph.id,
          "descripcion": this.description,
          "keywords": this.keywords
        }

        this.$apollo.mutate({
          mutation: CreateCategoria,
          variables: {
            input
          }
        })
        .then(() => {
          this.loading = false

          this.$router.push('/admin/categorias-productos')

          this.$toast.success('Categoría creada.', {
            duration : 3000
          })
        })
        .catch(() => {
          this.loading = false
        })
      }
    },
    computed: {
    }
  }
</script>

<style lang="scss">
.create-category {
}
</style>
