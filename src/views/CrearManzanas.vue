<template>
  <b-container fluid>
    <b-row align-h="center" class="mt-5">
      <b-col cols="12" md="6">
        <b-card class="transparent-card">
          <b-row class="text-center">
            <h1>Manzanas del Cuidado</h1>
          </b-row>
          <b-form @submit="agregarManzana">
            <!-- Campo para ingresar el código -->
            <b-form-group
              label="Código de la Manzana:"
              label-for="codigoInput"
            >
              <b-form-input
                id="codigoInput"
                v-model="nuevaManzana.codigo"
                required
              ></b-form-input>
            </b-form-group>

            <!-- Campo para ingresar el nombre -->
            <b-form-group
              label="Nombre de la Manzana:"
              label-for="nombreInput"
            >
              <b-form-input
                id="nombreInput"
                v-model="nuevaManzana.nombre"
                required
              ></b-form-input>
            </b-form-group>
            <b-form-group
              label="Localidad:"
              label-for="nombreInput"
            >
              <b-form-input
                id="nombreInput"
                v-model="nuevaManzana.localidad"
                required
              ></b-form-input>
            </b-form-group>
            <b-form-group
              label="Direccior:"
              label-for="nombreInput"
            >
              <b-form-input
                id="nombreInput"
                v-model="nuevaManzana.direccion"
                required
              ></b-form-input>
            </b-form-group>

            <!-- Campo para seleccionar el municipio -->
            
            <b-form-group
              label="Seleccionar un  Municipio:"
              label-for="nombreInput"
            >
              <b-form-select
                  id="municipioSelect"
                  v-model="nuevaManzana.municipio_id"
                  :options="municipios"
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
import router from '@/router';
import axios from 'axios'; // Importa la librería Axios
import CustomAlert from '@/components/Alert.vue'; // Reemplaza con la ubicación real de tu componente

export default {
  data() {
    return {
      nuevaManzana: {
        codigo: null,
        nombre: null,
        localidad:null,
        direccion:null,
        municipio_id: null,
      },
      municipios: null,
      showAlert: false,
      alertMessage: '',
      alertColor: 'success',
    };
  },
  methods: {
    listarMunicipios() {
      // Realiza la solicitud GET a la URL listar_municipios
      axios.get('listar_municipios')
        .then(response => {
          // Almacena los datos de las municipios en la variable municipios
          this.municipios = response.data;
          console.log(this.municipios)
        })
        .catch(error => {
          console.error('Error al obtener la lista de manzanas:', error);
        });
    },

    agregarManzana() {
      // Verifica si algún campo está vacío
      if (!this.nuevaManzana.codigo || !this.nuevaManzana.nombre || !this.nuevaManzana.localidad|| !this.nuevaManzana.direccion|| !this.nuevaManzana.municipio_id) {
        this.showAlertWithMessage('Ingrese un valor válido en todos los campos.', 'danger');
        return; // Detiene la función si hay campos nulos
      }
      console.log(this.nuevaManzana)


      // Realiza una solicitud POST al servidor para agregar la "manzana"
      axios
        .post('agregar_manzanas', this.nuevaManzana) // Reemplaza '/api/agregar-manzana' con tu endpoint de API real
        .then((response) => {
          // Maneja la respuesta del servidor
          console.log('', response.data);

          // Muestra la alerta de éxito
          this.showAlertWithMessage('Guardado', 'success');

          // Opcionalmente, puedes redirigir al usuario después de una adición exitosa
          router.push({ path: 'listado-manzanas' }); // Reemplaza con la ruta deseada
        })
        .catch((error) => {
          // Maneja cualquier error que pueda ocurrir durante la solicitud POST
          console.error('Error al agregar la Manzana:', error);

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
  mounted(){
    this.listarMunicipios()
  }
};
</script>

<style scoped>
/* Cambiar el color del botón Registrar a #005B8F */
.button{
  background-color: #005B8F !important;
}
</style>
