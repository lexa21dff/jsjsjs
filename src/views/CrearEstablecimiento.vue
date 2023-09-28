<template>
  <b-container fluid>
    <b-row align-h="center" class="mt-5">
      <b-col cols="12" md="6">
        <b-card class="transparent-card">
          <b-row class="text-center">
            <h1>Establecimientos</h1>
          </b-row>
          <b-form @submit="guardarEstablecimiento">
            <!-- Campo para ingresar el código del establecimiento -->
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

            <div class="m-2 text-center">
              <hr class="my-4">
              <b-button class="button" type="submit">Guardar</b-button>
            </div>

            <!-- Alerta de error o éxito -->
            <custom-alert
              v-if="showAlert"
              :message="alertMessage"
              :color="alertColor"
            />
          </b-form>
        </b-card>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
import axios from 'axios'; // Importa la librería Axios
import CustomAlert from '@/components/Alert.vue'; // Reemplaza con la ubicación real de tu componente

export default {
  data() {
    return {
      establecimiento: {
        codigo: '',
        nombre: '',
        responsable: '',
        direccion: '',
      },
      showAlert: false,
      alertMessage: '',
      alertColor: 'success',
    };
  },
  methods: {
    guardarEstablecimiento() {
      // Verifica si algún campo está vacío
      if (!this.establecimiento.codigo || !this.establecimiento.nombre || !this.establecimiento.responsable || !this.establecimiento.direccion) {
        this.showAlertWithMessage('Ingrese un valor válido en todos los campos.', 'danger');
        return; // Detiene la función si hay campos nulos
      }

      // Crea un nuevo objeto "establecimientoData" con los datos del formulario
      const establecimientoData = {
        codigo: this.establecimiento.codigo,
        nombre: this.establecimiento.nombre,
        responsable: this.establecimiento.responsable,
        direccion: this.establecimiento.direccion,
      };

      // Realiza una solicitud POST al servidor para agregar el "establecimiento"
      axios
        .post('agregar_establecimiento', establecimientoData) // Reemplaza '/api/agregar-establecimiento' con tu endpoint de API real
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
          this.showAlert= true
          // Muestra la alerta de error con el mensaje "Ingresaste un valor no válido"
          this.showAlertWithMessage('Ingresaste un valor no válido', 'danger');
        });
    },
    showAlertWithMessage(message, color) {
      this.alertMessage = message;
      this.alertColor = color;
      this.showAlert = true;
      setTimeout(() => {
        this.showAlert = false;
      }, 5000); // Oculta la alerta después de 5 segundos (ajustable)
    },
  },
  components: {
    CustomAlert,
  },
};
</script>

<style scoped>
/* Cambiar el color del botón Guardar a #005B8F */
.button {
  background-color: #005B8F !important;
}
</style>
