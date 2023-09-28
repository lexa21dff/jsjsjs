<template>
    <div>
      <b-button @click="mostrarModal">
        <b-icon icon="plus-lg"></b-icon>
      </b-button>
      <b-modal id="modal-1"  :title="idManzana" v-model="modalVisible" ok-title="Actualizar" cancel-title="Cancelar" @ok="actualizarManzana" @cancel="cancelarModal">
        <b-card class="transparent-card m-3" title="">
          <b-form>
            
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
           id:null,
          },
        };
      },
      methods: {
        mostrarModal() {
          // Cargar los datos de la manzana solo cuando se hace clic en el botón para abrir el modal
          this.modalVisible = true;
        },
        
        guardarEstablecimiento() {
  
        axios
        .post(`agregar_establecimiento_servicio/`, this.establecimiento) // Reemplaza '/api/agregar-establecimiento' con tu endpoint de API real
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