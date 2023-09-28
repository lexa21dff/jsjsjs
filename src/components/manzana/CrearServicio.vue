<template>
  <div>
    <b-button   variant="outline-dark m-1" @click="mostrarModal">
      <b-icon icon="plus-
"></b-icon>
      <span>Agregar</span>
    </b-button>

    <b-modal id="modal-1" title="" v-model="modalVisible" ok-title="Guardar" cancel-title="Cancelar" @ok="guardarServicio" @cancel="cancelarModal">
      <b-card class="transparent-card m-3" title="Servicio">
        <form @submit.prevent="agregarServicio">
          <b-form-group
              label="Seleccionar un Servicio:"
              label-for="nombreInput"
            >
              <b-form-select
                  id="municipioSelect"
                  v-model="servicio.codigo"
                  :options="manzana"
                  required
                  value-field="id"
                  text-field="nombre"
                  key-field="id"
                >
              </b-form-select>
            </b-form-group>
          </form>
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
      servicio: null,
      manzana:{
        id:null,
        nombre:null
      }
    };
  },
  methods: {
    mostrarModal() {
      this.listarServicios()
      this.cargarDatosManzana()
      this.modalVisible = true;
    },
    async listarServicios() {
      try {
        const response = await axios.get('listar_servicios'); // Reemplaza '/api/servicios' con la URL correcta de tu API
        this.servicios = response.data; // Almacena la lista de servicios en la variable servicios
      } catch (error) {
        console.error('Error al listar servicios:', error);
      }
    },
    cargarDatosManzana() {
      // Realizar una solicitud GET para obtener los datos de la manzana
      axios.get(`/listar_manzana/${this.idManzana}`)
        .then(response => {
          this.manzana.id = response.data.mensajes.id; 
          this.manzana.nombre = response.data.mensajes.nombre; 
        
        })
        .catch(error => {
          console.error('Error al cargar los datos de la manzana:', error);
        });
    },
    agregarManzanaServicio() {
      axios
      .post('agregar_servicio_manzana', this.servicio)
      .then((response) => {
        // El servicio se ha agregado correctamente, puedes mostrar una notificación o redirigir al usuario si es necesario.
        console.log('Servicio agregado con éxito:', response.data);
        
      })
      .catch((error) => {
        // Manejo de errores en caso de que la solicitud falle
        console.error('Error al agregar el servicio:', error);
      });
      this.modalVisible = false;
      this.router.push({ path: 'login' })
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
