<template>
<section id="subheader" class="subheader d-flex flex-column flex-md-row justify-content-between align-items-center" :class="headerFixed ? 'subheader--fixed' : ''">
    <h2 class="lead products__subtitle">{{ title }}</h2>

    <div class="d-none d-lg-flex">
        <div class="bg-light d-flex justify-content-center align-items-center px-3 pointer h-100" @click="mostrarCarrito()">
            <div class="h-100">
                <span class="icon d-flex justify-content-center align-items-center pr-3">
                    <i class="fas fa-shopping-cart"></i>
                </span>
            </div>
            <div class="h-100">
                <p class="text-muted my-0 d-flex">
                    <span class="font-weight-bold mr-2">{{ nroItemsCarrito }}</span>
                    items
                </p>
            <p class="my-0 font-weight-bold">Mi carrito</p>
            </div>
        </div>
        
        <!-- Ingresar o salir -->
        <div>
            <a href="" class="btn btn-danger rounded-0 text-white d-flex align-items-center w-100 h-100" @click.prevent="logout()" v-if="$route.name === 'mi-cuenta'">
                {{ loading ? 'Saliendo...' : 'Salir' }}
                <i class="fas fa-sign-out-alt ml-2"></i>
            </a>
            <a href="" class="btn btn-danger rounded-0 text-white d-flex align-items-center w-100 h-100" @click.prevent="toAccount()" v-else>Mi cuenta</a>
        </div>
    </div>
</section>
</template>

<script>
import { appConfig } from "@/env";

import { mapState } from 'vuex'

export default {
    data() {
        return {
            headerFixed: false,
            loading: false
        }
    },
    mounted() {
        this.$nextTick(function(){

            let _this = this

            // Setear color a links según posición de scroll
            const subheader = document.getElementById('subheader')

            // Posición del elemento
            if(subheader) {
                let coords = subheader.getBoundingClientRect();

                // Verifica si el usuario hizo scroll para mostrar el logo
                window.addEventListener("scroll", function(){
                    if(document.documentElement.scrollTop >= coords.top + 200) {
                        _this.headerFixed = true
                    }
                    else {
                        _this.headerFixed = false
                    }

                })
            }
        })
    },
    props: ['title'],
    methods: {
        mostrarCarrito() {
            // Abrir modal de carrito
            this.$store.commit('setModalCarrito', true)
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
        },
    },
    computed: {
        ...mapState(['nroItemsCarrito']),
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
.subheader {
    width: 100%;
    height: 50px;

    @media (min-width: 768px) {
      width: 100%;
    }

    &--fixed {
        width: 100%;

        background-color: white;
        
        @media (min-width: 768px) {
            width: 74.5%;
        }

        margin-left: auto;
        padding: .5rem 1rem;

        position: fixed;
        top: 0;
        right: 0;
        z-index: 100;
    }
}
</style>