<template>
  <b-container>
    <b-row align-h="center" class="mt-2">
      <b-col cols="12" md="6">
        <b-card class="transparent-card m-3" title="Servicio">
          <form @submit.prevent="agregarServicio">
            <!-- Campo para ingresar el nombre del servicio -->
            <b-form-group label="Nombre del Servicio" label-for="nombreInput">
              <b-form-input id="nombreInput" v-model="nuevoServicio.codigo" required></b-form-input>
            </b-form-group>
            <b-form-group label="Nombre del Servicio" label-for="nombreInput">
              <b-form-input id="nombreInput" v-model="nuevoServicio.nombre" required></b-form-input>
            </b-form-group>

            <!-- Campo para ingresar la descripción del servicio -->
            <b-form-group label="Descripción del Servicio" label-for="descripcionInput">
              <b-form-textarea id="descripcionInput" v-model="nuevoServicio.descripcion" required></b-form-textarea>
            </b-form-group>

            <!-- Campo para ingresar la fecha del servicio usando b-form-input -->
            <b-form-group label="Fecha del Servicio" label-for="fechaInput">
              <b-form-input type="date" id="fechaInput" v-model="nuevoServicio.fecha" required></b-form-input>
            </b-form-group>

            <!-- Campo para ingresar la hora del servicio -->
            <b-form-group label="Hora del Servicio" label-for="horaInput">
              <b-form-input id="horaInput" v-model="nuevoServicio.hora" type="time" required></b-form-input>
            </b-form-group>
            <b-form-group
              label="Seleccionar un  Municipio:"
              label-for="nombreInput"
            >
              <b-form-select
                  id="municipioSelect"
                  v-model="nuevoServicio.categorias"
                  :options="categorias"
                  required
                  value-field="id"
                  text-field="nombre"
                  key-field="id"
                >
              </b-form-select>
            </b-form-group>
            <div class="m-2 text-center">
              <hr class="my-4">
              <b-button class="button" type="submit">Guardar</b-button>
            </div>
          </form>
        </b-card>
        <div></div>
      </b-col>
    </b-row>
  </b-container>
</template>
<script>
import router from '@/router';
import moment from 'moment';
import axios from 'axios';
export default {
  data() {
    return {
      manzanas:null,
      nuevoServicio: {
        codigo: '',
        nombre: '',
        descripcion: '',
        fecha:null,
        hora:null,
      },
      categorias:null,
    };
  },
  methods: {
    agregarServicio() {
      console.log(this.nuevoServicio)
      this.nuevoServicio.fecha = moment(this.nuevoServicio.fecha).format('YYYY-MM-DD');
      axios
        .post('agregar_servicio', this.nuevoServicio)
        .then((response) => {
          // El servicio se ha agregado correctamente, puedes mostrar una notificación o redirigir al usuario si es necesario.
          console.log('Servicio agregado con éxito:', response.data);
          this.router.push('lista-servicios')
        })
        .catch((error) => {
          // Manejo de errores en caso de que la solicitud falle
          console.error('Error al agregar el servicio:', error);
        });
    },
    listarCategorias() {
      // Realiza la solicitud GET a la URL listar_Categorias
      axios.get('listar_categorias')
        .then(response => {
          // Almacena los datos de las Categorias en la variable Categorias
          this.categorias = response.data;
          console.log(this.categorias)
        })
        .catch(error => {
          console.error('Error al obtener la lista de manzanas:', error);
        });
    },
  },
  mounted(){
    this.listarCategorias()
  }

 
};
</script>

<style scoped>
/* Cambiar el color del botón Registrar a #005B8F */
.button {
  background-color: #005B8F !important;
}
</style>
