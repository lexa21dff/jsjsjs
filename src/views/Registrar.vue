<template>
  <b-container>
    <b-row>
      <b-col cols="6">
        <b-card class="m-2">
          <div class="text-center">
            <h1>Registrar</h1>
          </div>
          <div>
            <b-form action="" v-on:submit.prevent="guardarRegistroUsuario">
              <b-form-group label="Tipo de documento:">
                <b-form-select
                  v-model="form.tipo_documento"
                  :options="tipo_documento"
                  required
                  text-field="text"
                ></b-form-select>
              </b-form-group>
              <b-form-group
                id="input-tipo-documento"
                label="Documento:"
                label-for="input-tipo-documento"
              >
                <b-form-input
                  id="input-tipo-documento"
                  v-model="form.documento"
                  type="text"
                  placeholder="Documento"
                  required
                ></b-form-input>
              </b-form-group>
              <b-form-group
                id="input-nombres"
                label="Nombres completos:"
                label-for="input-nombres"
              >
                <b-form-input
                  type="text"
                  name="lname"
                  v-model="form.nombres"
                  placeholder="Ingrese su nombre"
                  class="form-control last"
                  required
                  minlength="4"
                  maxlength="35"
                  size="35"
                ></b-form-input>
              </b-form-group>
              <b-form-group
                id="input-apellidos"
                label="Apellidos Completos:"
                label-for="input-apellidos"
              >
                <b-form-input
                  id="input-apellidos"
                  v-model="form.apellidos"
                  placeholder="Apellidos Completos"
                  required
                ></b-form-input>
              </b-form-group>
              <b-form-group
                id="input-telefono"
                label="Telefono:"
                label-for="input-telefono"
              >
                <b-form-input
                  id="input-telefono"
                  v-model="form.telefono"
                  type="text"
                  placeholder="Telefono"
                  required
                ></b-form-input>
              </b-form-group>
              <b-form-group
                id="input-ciudad"
                label="Ciudad:"
                label-for="input-ciudad"
              >
                <b-form-input
                  id="input-ciudad"
                  v-model="form.ciudad"
                  placeholder="Ciudad"
                  required
                ></b-form-input>
              </b-form-group>
              <b-form-group
                id="input-direccion"
                label="Dirección:"
                label-for="input-direccion"
              >
                <b-form-input
                  id="input-direccion"
                  v-model="form.direccion"
                  placeholder="Direccion"
                  required
                ></b-form-input>
              </b-form-group>
              <b-form-group
                id="input-servicios"
                label="Servicios en los que te gustaría participar :"
                label-for="input-servicios"
              >
                <b-form-input
                  id="input-servicios"
                  v-model="form.servicios_ocupar"
                  placeholder="Servicios"
                  required
                ></b-form-input>
              </b-form-group>
              <b-form-group
                id="input-correo"
                label="Correo electrónico:"
                label-for="input-correo"
              >
                <b-form-input
                  id="input-correo"
                  v-model="form.correo_electronico"
                  type="email"
                  placeholder="Correo electrónico"
                  required
                ></b-form-input>
              </b-form-group>
              <b-form-group
                id="input-password"
                label="Contraseña:"
                label-for="input-password"
              >
                <b-form-input
                  id="input-password"
                  v-model="form.password"
                  type="password"
                  required
                ></b-form-input>
              </b-form-group>
              <b-form-group
                id="input-confirm-password"
                label="Confirmar contraseña:"
                label-for="input-confirm-password"
              >
                <b-form-input
                  id="input-confirm-password"
                  v-model="form.password_confirmation"
                  type="password"
                  required
                ></b-form-input>
              </b-form-group>
              <!-- Resto de tu formulario aquí -->
              <div class="m-3 text-center">
                <b-button
                  type="submit"
                  variant="primary"
                  v-on:click="guardarRegistroUsuario()"
                  >Registrar</b-button
                >
              </div>
            </b-form>
          </div>
        </b-card>
      </b-col>
    </b-row>
  </b-container>
</template>

  
  <script>
import axios from "axios";
export default {
  name: "Register",
  data() {
    return {
      tipo_documento: [
        { value: null, text: "Por favor seleccione una opción" },
        { value: "Tarjeta de identidad", text: "Tarjeta de identidad" },
        { value: "Cédula de ciudadanía", text: "Cédula de ciudadanía" },
      ],

      show: true,
      form: {
        tipo_documento: "",
        documento: "",
        nombres: "",
        apellidos: "",
        telefono: "",
        correo_electronico: "",
        ciudad: "",
        direccion: "",
        ocupacion: "",
        servicios_ocupar: "",
        password: "",
        password_confirmation: "",
      },
    };
  },
  methods: {
    onSubmit(event) {
      this.$router.push("/");
      event.preventDefault();
      alert(JSON.stringify(this.form));
    },
    onReset(event) {
      event.preventDefault();
      // Reset our form values
      this.form.email = "";
      this.form.name = "";
      this.form.food = null;
      this.form.checked = [];
      // Trick to reset/clear native browser form validation state
      this.show = false;
      this.$nextTick(() => {
        this.show = true;
      });
    },
    guardarRegistroUsuario() {
      axios
        .post("registro_usuario", this.form)
        .then((response) => {
          console.log(response.data);
        })
        .catch((error) => {
          // Manejar errores de la solicitud
          console.error("Error en la solicitud:", error);

          // Verificar si hay una respuesta del servidor
          if (error.response) {
            console.error("Respuesta del servidor:", error.response.data);
          }
        });
    },
  },
};
</script>
  <style>
</style>