<template>
  <div class="container">
    <div>
      
    </div>
    <div class="row text-center m-3">
      <h1>Listado de Establecimientos</h1>
    </div>
    <div class="mb-2">
      <b-input-group size="sm" class="mb-2">
        <b-input-group-prepend is-text>
          <b-icon icon="search"></b-icon>
        </b-input-group-prepend>
        <b-form-input type="search" v-model="searchQuery" placeholder="Buscar establecimientos"></b-form-input>
        <b-input-group-append>
          <b-button @click="buscarEstablecimientos">Buscar</b-button>
        </b-input-group-append>
      </b-input-group>
    </div>
    <table class="table table-striped table-hover m-4">
      <thead>
        <tr>
          <th scope="col">Nombre</th>
          <th scope="col">Acciones</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in establecimientos" :key="item.id" :value="item.id">
          <td>{{ item.nombre }}</td>
          <td>
            <div class="d-flex align-items-center">
              <DetalleEstablecimiento :idEstablecimiento="item.id"></DetalleEstablecimiento>
              <EditarEstablecimiento :idEstablecimiento="item.id"></EditarEstablecimiento>
              <Eliminar @confirmed="eliminarEstablecimiento(item.id)"></Eliminar>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from 'axios';
import Eliminar from '@/components/Eliminar.vue';
import DetalleEstablecimiento from '@/components/establecimientos/DetalleEstablecimiento.vue';
import EditarEstablecimiento from '@/components/establecimientos/editarEstablecimiento.vue';

export default {
  components: {
    Eliminar,
    DetalleEstablecimiento,
    EditarEstablecimiento
  },
  data() {
    return {
      establecimientos: null,
      searchQuery: '', // Agrega una propiedad para la consulta de búsqueda
    };
  },
  methods: {
    eliminarEstablecimiento(id) {
      console.log('Evento "confirmed" emitido, ID del establecimiento:', id);
      axios
        .delete(`eliminar_establecimiento/${id}`)
        .then((response) => {
          console.log('Establecimiento eliminado con éxito:', response.data);
          // Cierra el modal después de eliminar
          this.listarEstablecimientos();
          this.modalVisible = false;
        })
        .catch((error) => {
          // Manejo de errores
          console.log(error);
        });
    },
    listarEstablecimientos() {
      // Realiza la solicitud GET a la URL listar_establecimientos
      axios
        .get('listar_establecimientos')
        .then((response) => {
          // Almacena los datos de los establecimientos en la variable establecimientos
          this.establecimientos = response.data.mensaje;
          console.log(this.establecimientos);
        })
        .catch((error) => {
          console.error('Error al obtener la lista de establecimientos:', error);
        });
    },
    buscarEstablecimientos() {
      if (this.searchQuery.trim() !== '') {
        // Realiza la solicitud GET a la URL de búsqueda con el término de búsqueda
        axios
          .get(`buscador_establecimientos/${this.searchQuery}`)
          .then((response) => {
            // Actualiza la lista de establecimientos con los resultados de la búsqueda
            this.establecimientos = response.data;
          })
          .catch((error) => {
            console.error('Error al buscar establecimientos:', error);
          });
      } else {
        // Si el input está vacío, vuelve a cargar todos los establecimientos
        this.listarEstablecimientos();
      }
    },
  },
  mounted() {
    // Llama al método listarEstablecimientos cuando el componente se monta
    this.listarEstablecimientos();
  },
};
</script>
