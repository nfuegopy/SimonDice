
<template>
  <div class="container-fluid">
    <div class="row mt-3">
      <div class="col-12 col-md-10 offset-md-1 col-lg-6 offset-lg-3">
        <div class="card border-dark">
          <card class="card-header bg-info border-dark">
            <label v-if="nivel > 0">Nivel <b>{{ nivel }}</b> </label>
          </card>
          <card class="card-body">
            <div class="row row-cols-2">
              <colorVue class="bg-success" :color="'g'" @elegir="seleccion"></colorVue>
              <colorVue class="bg-danger" :color="'r'" @elegir="seleccion"></colorVue>
              <colorVue class="bg-warning" :color="'y'" @elegir="seleccion"></colorVue>
              <colorVue class="bg-primary" :color="'b'" @elegir="seleccion"></colorVue>
            </div>
          </card>
          <div class="row mt-3">
            <div class="col-12 col-md-6 offset-md-3">
              <div class="d-grid col-10 mx-auto mb-3">
                <button @click="jugar()" class="btn btn-dark" v-if="nivel === 0">
                  <i class="fa-solid fa-play"></i> Jugar
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>

    </div>

  </div>
</template>

<script>
import Swal from 'sweetalert2';
import colorVue from '../components/color.vue'
export default {
  components: { colorVue },
  data() {
    return { nivel: 0, colores: ['g', 'r', 'y', 'b'], simon: [], respuesta: [], turno: 'pc', indice: 0 }
  },
  methods: {
    seleccion(data) {
      if (this.turno === 'user') {
        this.respuesta.push(data);
        this.comparar();
      }
    },
    comparar() {
      if (this.respuesta[this.indice] === this.simon[this.indice]) {
        this.indice++;
        if (this.indice === this.nivel) {
          this.respuesta = [];
          this.simon = [];
          setTimeout(() => this.jugar(), 600);
        }
      } else {
        Swal.fire({
          title: '¡Ups!',
          text: '¡Lo siento, pero te has equivocado! Llegaste hasta el nivel ' + this.nivel + '. ¡Inténtalo de nuevo!',
          icon: 'error',
          customClass: {
            confirmButton: 'btn btn-primary',
            popup: 'animated zoomIn'
          },
          buttonsStyling: false
        });
        this.nivel = 0;
        this.indice = 0;
        this.respuesta = [];
        this.simon = [];
        this.turno = 'pc';
      }
    },
    aleatorio() {
      return Math.floor(Math.random() * 4);
    },
    jugar() {
      this.indice = 0;
      this.turno = 'pc';
      this.nivel++;
      for (let i = 1; i <= this.nivel; i++) {
        let num = this.colores[this.aleatorio()];
        this.simon.push(num);
        setTimeout(() => document.querySelector('#' + num).click(), 600 * (this.simon.length));
      }
      setTimeout(() => this.turno = 'user', 600 * (this.simon.length));
    }
  }
}
</script>

