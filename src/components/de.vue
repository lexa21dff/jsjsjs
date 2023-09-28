<template>
    <div>
      <b-button @click="mostrarModal">
        <b-icon icon="pencil-square"></b-icon>
      </b-button>
  
      <b-modal id="modal-1" title="Servicio" v-model="modalVisible" ok-title="Guardar" cancel-title="Cancelar" @ok="guardarServicio" @cancel="cancelarModal">
        <b-card class="transparent-card m-3" title="Servicio">
            <b-card>
        <b-card-title>Detalles de la Manzana</b-card-title>
        <b-card-text>
          <p><strong>ID:</strong> {{ manzana.id }}</p>
          <p><strong>Código:</strong> {{ manzana.codigo }}</p>
          <p><strong>Nombre:</strong> {{ manzana.nombre }}</p>
          <p><strong>Municipio ID:</strong> {{ manzana.municipio_id }}</p>
        </b-card-text>
      </b-card>
      </b-modal>
    </div>
  </template>

 
<script>
export default {
  props: ['idManzana'], // Declara una propiedad llamada idManzana
  data() {
    return {
      modalVisible: false,
      manzana: {},
    };
  },
  methods: {
    mostrarModal() {
      this.modalVisible = true;
    },
    
    guardarServicio() {
      // Lógica para guardar el servicio en la base de datos o realizar acciones relacionadas con "Guardar"
      // Por ejemplo:
      console.log('Guardando servicio...');
      // Cierra el modal después de guardar
      axios.get(`/api/manzanas/${this.idManzana}`)
        .then(response => {
          // Actualizar los detalles de la manzana con los datos de la respuesta
          this.manzana = response.data;
        })
        .catch(error => {
          console.error('Error al obtener detalles de la manzana:', error);
        });
      this.modalVisible = false;
    },
    cancelarModal() {
      // Lógica para cancelar o cerrar el modal sin guardar
      // Por ejemplo:
      console.log('Cerrando modal sin guardar...');
      // Cierra el modal sin guardar cambios
      this.modalVisible = false;
    },
  },
};
</script>

<style scoped>
/* Cambiar el color del botón Registrar a #005B8F */
.button {
  background-color: #005B8F !important;
}
</style>

