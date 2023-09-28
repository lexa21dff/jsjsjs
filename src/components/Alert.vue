<template>
  <b-alert
    :variant="color"
    dismissible
    fade
    :show="showAlert"
    @dismissed="showAlert = false"
  >
    {{ message }}
  </b-alert>
</template>

<script>
export default {
  props: {
    message: String, // Propiedad para el mensaje de la alerta
    color: String,   // Propiedad para el color de la alerta
  },
  data() {
    return {
      showAlert: false,
    };
  },
  watch: {
    message: 'showAlertMessage', // Escucha cambios en la propiedad 'message'
  },
  methods: {
    showAlertMessage() {
      // Muestra la alerta cuando se actualiza el mensaje
      this.showAlert = true;
      setTimeout(() => {
        this.showAlert = false;
      }, 5000); // Oculta la alerta después de 5 segundos (ajustable)
    },
  },
};
</script>
