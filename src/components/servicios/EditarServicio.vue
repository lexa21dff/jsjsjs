<template>
  <div>
    <b-button   variant="outline-dark m-1" @click="mostrarModal">
      <b-icon icon="pencil-square"></b-icon>
      <span>Editar</span>
    </b-button>

    <b-modal id="modal-1" title="Actualizar Servicio" v-model="modalVisible" ok-title="Actualizar" cancel-title="Cancelar" @ok="guardarServicio" @cancel="cancelarModal">
      <b-card class="transparent-card m-3" title="Servicio">
        <!-- Aquí puedes mostrar el valor de idManzana -->

        <form >
            <!-- Campo para ingresar el nombre del servicio -->
            <b-form-group label="Codigo del Servicio" label-for="nombreInput">
              <b-form-input id="nombreInput" v-model="servicio.codigo" required></b-form-input>
            </b-form-group>
            <b-form-group label="Nombre del Servicio" label-for="nombreInput">
              <b-form-input id="nombreInput" v-model="servicio.nombre" required></b-form-input>
            </b-form-group>

            <!-- Campo para ingresar la descripción del servicio -->
            <b-form-group label="Descripción del Servicio" label-for="descripcionInput">
              <b-form-textarea id="descripcionInput" v-model="servicio.descripcion" required></b-form-textarea>
            </b-form-group>

            <!-- Campo para ingresar la fecha del servicio usando b-form-input -->
            <b-form-group label="Fecha del Servicio" label-for="fechaInput">
              <b-form-input type="date" id="fechaInput" v-model="servicio.fecha" required></b-form-input>
            </b-form-group>

            <!-- Campo para ingresar la hora del servicio -->
            <b-form-group label="Hora del Servicio" label-for="horaInput">
              <b-form-input id="horaInput" v-model="servicio.hora" type="time" required></b-form-input>
            </b-form-group>

          </form>
      </b-card>
    </b-modal>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  props: ['idServicio'], // Declara una propiedad llamada idManzana
  data() {
    return {
      modalVisible: false,
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
        this.cargarDatosServicios()
          this.modalVisible = true;
      },
      cargarDatosServicios() {
      // Realizar una solicitud GET para obtener los datos de la manzana
        axios.get(`listar_servicio/${this.idServicio}`)
        .then(response => {
          console.log(response)
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
