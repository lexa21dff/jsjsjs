<template>
    <div>
      <b-button @click="mostrarModal">
        <b-icon icon="pencil-square"></b-icon>
      </b-button>
  
      <b-modal id="modal-1" title="Servicio" v-model="modalVisible" ok-title="Guardar" cancel-title="Cancelar" @ok="guardarServicio" @cancel="cancelarModal">
        <b-card class="transparent-card m-3" title="Servicio">
          <form @submit.prevent="agregarServicio">
            <b-form-group
                label="Seleccionar una Manzana:"
                label-for="nombreInput"
              >
                <b-form-select
                    id="municipioSelect"
                    v-model="nuevoServicio.codigo"
                    :options="manzanas"
                    required
                    value-field="id"
                    text-field="nombre"
                    key-field="id"
                  >
                </b-form-select>
              </b-form-group>

              <!-- Campo para ingresar el nombre del servicio -->
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

              <div class="m-2 text-center">
                <hr class="my-4">
                <b-button class="button" type="submit">Guardar</b-button>
              </div>
            </form>
        </b-card>
      </b-modal>
    </div>
  </template>
  
  <script>
  export default {
    props: ['idServicio'], // Declara una propiedad llamada idManzana
    data() {
      return {
        modalVisible: false,
        nuevoServicio: {
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
        this.modalVisible = true;
      },
      listarManzanas() {
      // Realiza la solicitud GET a la URL listar_manzanas
      axios
        .get('listar_manzanas')
        .then((response) => {
          // Almacena los datos de las manzanas en la variable manzanas
          this.manzanas = response.data;
          console.log(this.manzanas)
        })
        .catch((error) => {
          console.error('Error al obtener la lista de manzanas:', error);
        });
    },
      agregarServicio() {
        axios
        .put('actualizar_servicio', this.nuevoServicio)
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
    
    mounted(){
      this.listarManzanas()
    }
  };
  </script>
  
  <style scoped>
  /* Cambiar el color del botón Registrar a #005B8F */
  .button {
    background-color: #005B8F !important;
  }
  </style>
  