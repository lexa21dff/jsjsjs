<template>
  <div>
    <b-button @click="mostrarModal">
      <b-icon icon="eye"></b-icon>
    </b-button>

    <b-modal id="modal-1" title="Detalle del Servicio" v-model="modalVisible" ok-title="Guardar" cancel-title="Cancelar" @ok="guardarServicio" @cancel="cancelarModal">
      <b-card class="transparent-card m-3" :title="manzana">
          <b-card-text>
            <p><strong>Código del servicio:</strong> {{ servicio.codigo }}</p>
            <p><strong>Nombre del servicio:</strong> {{ servicio.nombre }}</p>
            <p><strong>Descripción del servicio:</strong> {{ servicio.descripcion }}</p>
            <p><strong>Fecha del Servicio:</strong> {{ servicio.descripcion }}</p>
            <p><strong>Hora del Servicio:</strong> {{ servicio.descripcion }}</p>

          </b-card-text>   
      </b-card>
    </b-modal>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  props: ['idServicio'],
  data() {
    return {
      modalVisible: false,
      manzana: null,
      servicio: {
      codigo: '',
      nombre: '',
      descripcion: '',
      fecha:null,
      hora:null,
    },
    };
  },
  methods: {
    mostrarModal() {
      // Cargar los datos de la manzana solo cuando se hace clic en el botón para abrir el modal
      this.cargarDatosServicio()
      this.modalVisible = true;
    },
    cargarDatosServicio() {
      // Realizar una solicitud GET para obtener los datos de la manzana
      axios.get(`listar_servicio/${this.idServicio}`)
        .then(response => {
          this.servicio.codigo = response.data.mensajes.codigo; 
          this.servicio.nombre = response.data.mensajes.nombre; 
          this.servicio.descripcion = response.data.mensajes.descripcion;
          this.servicio.fecha = response.data.mensajes.fecha;
          this.servicio.hora = response.data.mensajes.hora;
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
  mounted(){
    this.servicio()
  }
};
</script>