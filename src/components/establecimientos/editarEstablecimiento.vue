<template>
  <div>
    <b-button   variant="outline-dark m-1" @click="mostrarModal">
      <b-icon icon="pencil-square"></b-icon>
      <span>Editar</span>
    </b-button>
    <b-modal id="modal-1"  title="Actualizar establecimiento" v-model="modalVisible" ok-title="Actualizar" cancel-title="Cancelar" @ok="actualizarManzana" @cancel="cancelarModal">
      <b-card class="transparent-card m-3" title="">
        <b-form>
          <b-form-group
              label="Código del establecimiento"
              label-for="codigoInput"
            >
              <b-form-input
                id="codigoInput"
                v-model="establecimiento.codigo"
                required
              ></b-form-input>
            </b-form-group>

            <!-- Campo para ingresar el nombre del establecimiento -->
            <b-form-group
              label="Nombre del establecimiento"
              label-for="nombreInput"
            >
              <b-form-input
                id="nombreInput"
                v-model="establecimiento.nombre"
                required
              ></b-form-input>
            </b-form-group>

            <!-- Campo para ingresar el nombre del responsable -->
            <b-form-group
              label="Nombre del responsable"
              label-for="responsableInput"
            >
              <b-form-input
                id="responsableInput"
                v-model="establecimiento.responsable"
                required
              ></b-form-input>
            </b-form-group>

            <!-- Campo para ingresar la dirección del establecimiento -->
            <b-form-group
              label="Dirección del establecimiento"
              label-for="direccionInput"
            >
              <b-form-input
                id="direccionInput"
                v-model="establecimiento.direccion"
                required
              ></b-form-input>
            </b-form-group>

        </b-form>
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
        // Cargar los datos de la manzana solo cuando se hace clic en el botón para abrir el modal
        this.cargarDatosEstablecimiento();
        this.modalVisible = true;
      },
      cargarDatosEstablecimiento() {
        // Realizar una solicitud GET para obtener los datos de la manzana
        axios.get(`listar_establecimiento/${this.idEstablecimiento}`)
          .then(response => {
            this.establecimiento.codigo = response.data.mensajes.codigo; 
            this.establecimiento.nombre = response.data.mensajes.nombre; 
            this.establecimiento.direccion = response.data.mensajes.direccion;
            this.establecimiento.responsable = response.data.mensajes.responsable;           
          })
          .catch(error => {
            console.error('Error al cargar los datos de la manzana:', error);
          });
      },
      guardarEstablecimiento() {

      axios
      .put(`actualizar_establecimiento/${this.idEstablecimiento}`, this.establecimiento) // Reemplaza '/api/agregar-establecimiento' con tu endpoint de API real
        .then((response) => {
          // Maneja la respuesta del servidor
          console.log('Establecimiento agregado:', response.data);

          // Muestra la alerta de éxito
          this.showAlertWithMessage('Guardado', 'success');

          // Opcionalmente, puedes redirigir al usuario después de una adición exitosa
          router.push({ path: 'login' }); // Reemplaza con la ruta deseada
        })
        .catch((error) => {
          // Maneja cualquier error que pueda ocurrir durante la solicitud POST
          console.error('Error al agregar el Establecimiento:', error);
          
        });
    },

      cancelarModal() {
        // Cierra el modal sin guardar cambios
        this.modalVisible = false;
      },
    },
  };
  </script>