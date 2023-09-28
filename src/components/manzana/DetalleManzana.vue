<template>
  <div>
    <b-button  class="m-1"  variant="outline-dark" @click="mostrarModal">
      <b-icon icon="eye"></b-icon>
      <span>Ver</span>
    </b-button>

    <b-modal id="modal-1" :title="manzana.nombre" v-model="modalVisible" ok-title="Guardar" cancel-title="Cancelar" @ok="guardarServicio" @cancel="cancelarModal">
      <b-card class="transparent-card m-3" >
          <b-card-text>
              <p><strong>Código:</strong> {{ manzana.codigo }}</p>
              <p><strong>Municipio :</strong> {{ manzana.municipio }}</p>
              <p><strong>localidad:</strong> {{ manzana.localidad }}</p>
              <p><strong>Direccion:</strong> {{ manzana.direccion }}</p>
          </b-card-text>   
      </b-card>
    </b-modal>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  props: ['idManzana'],
  data() {
    return {
      manzanaActualizada: false, 
      modalVisible: false,
      municipios: null,
      manzana: {
        codigo: null,
        nombre: null,
        localidad: null,
        direccion: null,
        municipio: null,
      },
    };
  },
  methods: {
    mostrarModal() {
      // Cargar los datos de la manzana solo cuando se hace clic en el botón para abrir el modal
      this.cargarDatosManzana();
      this.listarMunicipios()
      this.modalVisible = true;
    },
    async listarMunicipios() {
      // Realiza la solicitud GET a la URL listar_municipios
      await axios.get('listar_municipio')
        .then(response => {
          // Almacena los datos de las municipios en la variable municipios
          this.municipios = response.data;
        })
        .catch(error => {
          console.error('Error al obtener la lista de manzanas:', error);
        });
    },
    cargarDatosManzana() {
      // Realizar una solicitud GET para obtener los datos de la manzana
      axios.get(`/listar_manzana/${this.idManzana}`)
        .then(response => {
          this.manzana.codigo = response.data.mensajes.codigo; 
          this.manzana.nombre = response.data.mensajes.nombre; 
          this.manzana.direccion = response.data.mensajes.direccion;
          this.manzana.localidad = response.data.mensajes.localidad;
          this.manzana.municipio = response.data.mensajes.municipio;
        })
        .catch(error => {
          console.error('Error al cargar los datos de la manzana:', error);
        });
    },

    cancelarModal() {
      // Cierra el modal sin guardar cambios
      this.modalVisible = false;
    },
  },
};
</script>