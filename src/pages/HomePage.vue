<template>
  <q-page class="home-page">
    <div class="welcome-container">
      <h1 class="welcome-message">BIENVENIDO AL PORTAL DE LA NASA</h1>
      <q-btn label="Ingresar" color="primary" @click="goToApod" />
    </div>
    <img
      src="https://graffica.info/wp-content/uploads/2017/07/logotipo-nasa.jpg"
      class="background-image"
    />
  </q-page>
</template>

<script>
export default {
  name: "HomePage",
  methods: {
    goToApod() {
      this.$router.push("/apod");
    },
  },
};
</script>

<style scoped>
.home-page {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  position: relative;
  background-color: #fff;
  color: #000;
}

.welcome-container {
  text-align: center;
  color: #000; /* Texto blanco */
}

.welcome-message {
  font-size: 5rem; /* Aumenta el tamaño del texto */
  margin-bottom: 50px;
}

.background-image {
  position: absolute;
  top: 20px;
  right: 20px;
  width: 300px; /* Ajusta el tamaño de la imagen según sea necesario */
  height: auto;
}
</style>
