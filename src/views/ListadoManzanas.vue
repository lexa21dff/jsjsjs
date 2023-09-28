<template>
  <div class="container">
    <div class="row m-10"></div>
    <div class="row text-center">
      <h1>Listado de Manzanas</h1>
    </div>
    <div>
      <b-input-group size="sm" class="mb-2">
        <b-form-input type="search" v-model="searchQuery" placeholder="Buscar manzanas"></b-form-input>
        <b-input-group-append is-text>
          <b-icon icon="search" @click="buscarManzanas"></b-icon>
        </b-input-group-append>
      </b-input-group>
    </div>
    <table class="table table-striped table-hover">
      <thead>
        <tr>
          <th scope="col">Manzana</th>
          <th scope="col">Servicios</th>
          <th scope="col">Acciones</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in manzanas" :key="item.id" :value="item.id">
          <td>{{ item.nombre }}</td>
          <td>
            <!-- Columna para CrearServicio -->
            <div class="d-flex justify-content-between align-items-center">
              <CrearServicio :idManzana="item.id_manzana"></CrearServicio>
            </div>
          </td>
          <td>
            <!-- Columna para los botones de acciones -->
            <div class="d-flex align-items-center">
              <DetalleManzana :idManzana="item.id_manzana"></DetalleManzana>
              <EditarManzana :idManzana="item.id_manzana"></EditarManzana>
              <Eliminar @confirmed="eliminarManzana(item.id)"></Eliminar>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import CrearServicio from '@/components/manzana/CrearServicio.vue';
import DetalleManzana from '@/components/manzana/DetalleManzana.vue';
import EditarManzana from '@/components/manzana/EditarManzana.vue';
import Eliminar from '@/components/Eliminar.vue';
import axios from 'axios';

export default {
  components: {
    CrearServicio,
    DetalleManzana,
    EditarManzana,
    Eliminar,
  },
  data() {
    return {
      showText: '',
      manzanas: [],
      searchQuery: '', // Agrega una propiedad para la consulta de búsqueda
    };
  },
  methods: {
    eliminarManzana(id) {
      console.log('Evento "confirmed" emitido, ID de la manzana:', id);
      axios
        .delete(`eliminar_manzana/${id}`)
        .then((response) => {
          console.log('Manzana eliminada con éxito:', response.data);
          // Cierra el modal después de eliminar
          this.listarManzanas();
          this.modalVisible = false;
        })
        .catch((error) => {
          // Manejo de errores
          console.log(error);
        });
    },
    listarManzanas() {
      // Realiza la solicitud GET a la URL listar_manzanas
      axios
        .get('listar_manzanas')
        .then((response) => {
          // Almacena los datos de las manzanas en la variable manzanas
          this.manzanas = response.data;
        })
        .catch((error) => {
          console.error('Error al obtener la lista de manzanas:', error);
        });
    },
    buscarManzanas() {
      if (this.searchQuery.trim() !== '') {
        axios
          .get(`buscador_manzanas/${this.searchQuery}`)
          .then((response) => {
            this.manzanas = response.data;
          })
          .catch((error) => {
            console.error('Error al buscar manzanas:', error);
          });
      } else {
        // Si el input está vacío, vuelve a cargar todas las manzanas
        this.listarManzanas();
      }
    },
  },
  watch: {
    // Observa la propiedad manzanaActualizada del componente hijo
    'EditarManzana.manzanaActualizada': function (nuevoValor) {
      if (nuevoValor) {
        // Cuando manzanaActualizada cambia a true en el componente hijo,
        // llama al método para actualizar la lista de manzanas
        this.actualizarListaManzanas();

        // Restablece la propiedad manzanaActualizada en el componente hijo a false
        this.$refs.editarManzana.manzanaActualizada = false;
      }
    },
  },
  mounted() {
    // Llama al método listarManzanas cuando el componente se monta
    this.listarManzanas();
  },
};
</script>
