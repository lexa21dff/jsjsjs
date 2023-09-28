<template>
    <div>
      <b-button  class="m-1" variant="outline-danger" @click="showConfirmationDialog">
        <b-icon icon="trash"></b-icon>
        <span>Eliminar</span>
      </b-button>
    </div>
  </template>
  
  <script>
  export default {
    methods: {
      showConfirmationDialog() {
        this.$swal({
          title: '¿Estás seguro?',
          text: '¿Realmente quieres eliminar este elemento?',
          icon: 'warning',
          showCancelButton: true,
          confirmButtonText: 'Sí, eliminar',
          cancelButtonText: 'Cancelar',
        }).then((result) => {
          if (result.isConfirmed) {
            this.$emit('confirmed'); // Emitir un evento al componente padre si se confirma la eliminación
          }
        });
      },
    },
  };
  </script>
  