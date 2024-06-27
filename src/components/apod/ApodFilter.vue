<template>
  <q-card flat bordered>
    <q-card-section>
      <q-form @submit.prevent="fetchData">
        <div class="q-gutter-md">
          <q-input v-model="startDate" label="Fecha Inicio (YYYY-MM-DD)" type="date" />
          <q-input v-model="endDate" label="Fecha Fin (YYYY-MM-DD)" type="date" />
          <q-input v-model="searchTerm" label="Buscar en Descripción" type="text" />
          <q-btn label="Consultar" type="submit" color="primary" />
        </div>
      </q-form>
    </q-card-section>
  </q-card>
</template>

<script>
export default {
  name: 'ApodFilter',
  emits: ['fetchData'],
  data() {
    return {
      startDate: '',
      endDate: '',
      searchTerm: ''
    };
  },
  methods: {
    fetchData() {
      this.$emit('fetchData', {
        startDate: this.startDate,
        endDate: this.endDate,
        searchTerm: this.searchTerm
      });
    }
  }
};
</script>
