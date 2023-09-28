<!-- AgregarUbicacionMapa.vue -->
<template>
    <div>
      <h1>Agregar Ubicación</h1>
      <div class="map-container">
        <gmap-map
          :center="mapCenter"
          :zoom="mapZoom"
          @click="onMapClick"
        >
          <!-- Marca para indicar la ubicación seleccionada -->
          <gmap-marker
            :position="selectedLocation"
            :draggable="true"
            @dragend="onMarkerDragEnd"
          ></gmap-marker>
        </gmap-map>
      </div>
      <div class="location-info">
        <b-form @submit="agregarUbicacion">
          <b-form-group label="Latitud" label-for="latitud">
            <b-form-input id="latitud" v-model="latitud" type="text" required></b-form-input>
          </b-form-group>
          <b-form-group label="Longitud" label-for="longitud">
            <b-form-input id="longitud" v-model="longitud" type="text" required></b-form-input>
          </b-form-group>
          <b-button type="submit" variant="primary">Agregar Ubicación</b-button>
        </b-form>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        mapCenter: { lat: 0, lng: 0 },
        mapZoom: 10,
        selectedLocation: null,
        latitud: "",
        longitud: "",
      };
    },
    methods: {
      onMapClick(event) {
        // Actualiza la ubicación seleccionada al hacer clic en el mapa
        this.selectedLocation = {
          lat: event.latLng.lat(),
          lng: event.latLng.lng(),
        };
      },
      onMarkerDragEnd(event) {
        // Actualiza la ubicación seleccionada cuando se arrastra el marcador
        this.selectedLocation = {
          lat: event.latLng.lat(),
          lng: event.latLng.lng(),
        };
      },
      agregarUbicacion() {
        // Procesa la ubicación seleccionada, por ejemplo, guardándola en una lista o enviándola a tu API
        const ubicacion = {
          latitud: this.selectedLocation.lat,
          longitud: this.selectedLocation.lng,
        };
        console.log("Nueva ubicación agregada:", ubicacion);
      },
    },
  };
  </script>
  
  <style scoped>
  .map-container {
    height: 400px;
  }
  
  .location-info {
    margin-top: 20px;
  }
  </style>
  