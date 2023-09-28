<template>
  <div>
    <b-button   variant="outline-dark m-1" @click="mostrarModal">
      <b-icon icon="pencil-square"></b-icon>
      <span>Editar</span>
    </b-button>
    <b-modal id="modal-1"  :title="idManzana" v-model="modalVisible" ok-title="Actualizar" cancel-title="Cancelar" @ok="actualizarManzana" @cancel="cancelarModal">
      <b-card class="transparent-card m-3" title="">
        <b-form>
          <!-- <b-form-group
              label="Código de la Manzana:"
              label-for="codigoInput"
            >
              <b-form-input
                id="codigoInput"
                v-model="manzana.codigo"
                required
              ></b-form-input>
          </b-form-group> -->
          <b-form-group
              label="Nombre de la Manzana:"
              label-for="nombreInput"
            >
              <b-form-input
                id="nombreInput"
                v-model="manzana.nombre"
                required
              ></b-form-input>
            </b-form-group>
            <b-form-group
              label="Localidad:"
              label-for="nombreInput"
            >
              <b-form-input
                id="nombreInput"
                v-model="manzana.localidad"
                required
              ></b-form-input>
            </b-form-group>
            <b-form-group
              label="Direccior:"
              label-for="nombreInput"
            >
              <b-form-input
                id="nombreInput"
                v-model="manzana.direccion"
                required
              ></b-form-input>
            </b-form-group>
            <b-form-group
              label="Seleccionar un  Municipio:"
              label-for="nombreInput"
            >
              <b-form-select
                  id="municipioSelect"
                  v-model="manzana.municipio_id"
                  :options="municipios"
                  required
                  value-field="id"
                  text-field="nombre"
                  key-field="id"
                >
              </b-form-select>
            </b-form-group>
        </b-form>
      </b-card>
    </b-modal>
  </div>
</template>
<script>
import axios from 'axios';

export default {
  props: ['idManzana'],
  data() {
    return {
      manzanaActualizada: false, 
      modalVisible: false,
      municipios: null,
      manzana: {
        codigo: null,
        nombre: null,
        localidad: null,
        direccion: null,
        municipio_id: null,
      },
    };
  },
  methods: {
    mostrarModal() {
      // Cargar los datos de la manzana solo cuando se hace clic en el botón para abrir el modal
      this.cargarDatosManzana();
      this.listarMunicipios()
      this.modalVisible = true;
    },
    async listarMunicipios() {
      // Realiza la solicitud GET a la URL listar_municipios
      await axios.get('listar_municipio')
        .then(response => {
          // Almacena los datos de las municipios en la variable municipios
          this.municipios = response.data;
          console.log(this.municipios)
        })
        .catch(error => {
          console.error('Error al obtener la lista de manzanas:', error);
        });
    },
    cargarDatosManzana() {
      // Realizar una solicitud GET para obtener los datos de la manzana
      axios.get(`listar_manzana/${this.idManzana}`)
        .then(response => {
          console.log(response)
          this.manzana.codigo = response.data.mensajes.codigo; 
          this.manzana.nombre = response.data.mensajes.nombre; 
          this.manzana.direccion = response.data.mensajes.direccion;
          this.manzana.localidad = response.data.mensajes.localidad;
          this.manzana.municipio = response.data.mensajes.municipio;

        })
        .catch(error => {
          console.error('Error al cargar los datos de la manzana:', error);
        });
    },
    actualizarManzana() {
  // Realizar una solicitud PUT para actualizar los datos de la manzana
      console.log(this.manzana);
      axios
        .put(`actualizar_manzana/${this.idManzana}`, this.manzana)
        .then((response) => {
          console.log('Manzana actualizada con éxito:', response.data);
          // Cierra el modal después de actualizar
          this.modalVisible = false;
          // Emitir un evento personalizado para indicar que la actualización se ha completado
        })
        .catch((error) => {
          // Manejo de errores
          console.log(error);
        });
},
    cancelarModal() {
      // Cierra el modal sin guardar cambios
      this.modalVisible = false;
    },
  },
};
</script>