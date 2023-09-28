<template>
  <div>
    <b-button @click="mostrarModal">
      <b-icon icon="eye"></b-icon>
    </b-button>

    <b-modal id="modal-1" title="Servicio" v-model="modalVisible" ok-title="Guardar" cancel-title="Cancelar" @ok="guardarServicio" @cancel="cancelarModal">
      <b-card class="transparent-card m-3" title="Manzanas cuidadoras">
          <b-card-text>
            <p><strong>ID:</strong> {{ establecimiento.id }}</p>
            <p><strong>Código:</strong> {{ establecimiento.codigo }}</p>
            <p><strong>Nombre:</strong> {{ establecimiento.nombre }}</p>
            <p><strong>Responsable:</strong> {{ establecimiento.responsable }}</p>
            <p><strong>Dirección:</strong> {{ establecimiento.direccion }}</p>
          </b-card-text>   
      </b-card>
    </b-modal>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  props: ['idEstablecimiento'],
  data() {
    return {
      manzanaActualizada: false, 
      modalVisible: false,
      establecimiento:
      {
        codigo: "",
        nombre: "",
        responsable: "",
        direccion: "",
      },
    };
  },
  methods: {
    mostrarModal() {
      this.cargarDatosEstablecimiento();
      this.modalVisible = true;
    },
    cargarDatosEstablecimiento() {
      console.log(this.idEstablecimiento)
      axios.get(`listar_establecimiento/${this.idEstablecimiento}`)
        .then(response => {
          
          console.log(response)
          this.establecimiento.codigo = response.data.mensajes.codigo; 
          this.establecimiento.nombre = response.data.mensajes.nombre; 
          this.establecimiento.direccion = response.data.mensajes.direccion;
          this.establecimiento.responsable = response.data.mensajes.responsable;           
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