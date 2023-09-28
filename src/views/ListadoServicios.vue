<template>
    <div class="container">
        <div class="row text-center">
            <h1>Listado de Servicios</h1>
        </div>
        <div>
          <b-input-group size="sm" class="mb-2">
            <b-input-group-prepend is-text>
              <b-icon icon="search"></b-icon>
            </b-input-group-prepend>
            <b-form-input type="search" placeholder="Buscar servicios"></b-form-input>
          </b-input-group>
        </div>
        <table class="table table-striped table-hover">
          <thead>
            <tr>
              <th scope="col">Nombre</th>
              <th scope="col">Descripcion</th> 
              <th scope="col">Establecimiento</th> 
              <th scope="col">acciones</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="item in servicios" :key="item.id" :value="item.id">
              <td>
                {{ item.nombre }}
              </td>
              <td>
                {{ item.descripcion }}
              </td>
              <td>
                <button class="btn btn-outline-secondary">
                  Registrar
                </button>
                <b-button class="m-1" variant="outline-primary">
                  <b-icon icon="eye">
                  </b-icon>
                </b-button>
              </td>
              <td>
                <div class="d-flex align-items-center">
                  <DetalleServicio :idServicio="item.id_servicio"></DetalleServicio>
                  <EditarServicio :idServicio="item.id_servicio"></EditarServicio>
                  <Eliminar @confirmed="eliminarServicio(item.id_servicio)"></Eliminar>
            </div>
              </td>
            </tr>
          </tbody>
        </table>
    </div>
</template>

<script>
import Eliminar from '@/components/Eliminar.vue'
import EditarServicio from '@/components/servicios/EditarServicio.vue'
import DetalleServicio from '@/components/servicios/DetalleServicios.vue';
import axios from 'axios';

export default {
  components:{
    Eliminar,
    DetalleServicio,
    EditarServicio
  },
  data() {
    return {
      servicios: [], // Aquí almacenaremos la lista de servicios
    };
  },
  methods: {
    // Método para listar servicios desde tu API
    async listarServicios() {
      try {
        const response = await axios.get('listar_servicios'); // Reemplaza '/api/servicios' con la URL correcta de tu API
        this.servicios = response.data; // Almacena la lista de servicios en la variable servicios
      } catch (error) {
        console.error('Error al listar servicios:', error);
      }
    },
    
    // Método para eliminar un servicio
    async eliminarServicio(id) {
      try {
        const response = await axios.delete(`eliminar_servicio/${id}`); // Reemplaza '/api/servicios' con la URL correcta de tu API
        console.log('Servicio eliminado con éxito:', response.data);
        // Actualiza la lista de servicios después de eliminar
        this.listarServicios();
      } catch (error) {
        console.error('Error al eliminar servicio:', error);
      }
    },
  },
  mounted() {
    // Llama al método listarServicios cuando el componente se monta
    this.listarServicios();
  },
};
</script>