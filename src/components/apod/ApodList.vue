<template>
  <q-page padding class="bg-page">
    <div class="q-pa-md row q-col-gutter-md">
      <div class="col-2 filter-container">
        <q-card flat bordered>
          <q-card-section>
            <q-form @submit.prevent="fetchApodData">
              <div class="q-gutter-md">
                <q-input v-model="startDate" label="Fecha Inicio (YYYY-MM-DD)" type="date" />
                <q-input v-model="endDate" label="Fecha Fin (YYYY-MM-DD)" type="date" />
                <q-input v-model="searchTerm" label="Buscar en Descripción" type="text" />
                <q-btn label="Consultar" type="submit" color="primary" />
              </div>
            </q-form>
          </q-card-section>
        </q-card>
      </div>
      <div class="col-10 content-container">
        <div class="q-pa-md row items-start q-gutter-md">
          <q-card v-for="item in filteredApodData" :key="item.date" class="my-card" flat bordered>
            <template v-if="item.media_type === 'image'">
              <q-img :src="item.url">
                <div class="absolute-bottom text-subtitle2 text-center">
                  {{ item.title }}
                </div>
              </q-img>
            </template>
            <template v-else>
              <q-video :src="item.url" />
            </template>
            <q-card-section>
              <div class="text-h6">{{ item.title }}</div>
              <div class="text-subtitle2">Fecha: {{ item.date }}</div>
            </q-card-section>
            <q-card-actions>
              <q-space />
              <q-btn
                color="grey"
                round
                flat
                dense
                :icon="expanded[item.date] ? 'keyboard_arrow_up' : 'keyboard_arrow_down'"
                @click="toggleExpand(item.date)"
              />
            </q-card-actions>
            <q-slide-transition>
              <div v-show="expanded[item.date]">
                <q-separator />
                <q-card-section class="text-subtitle2">
                  {{ item.explanation }}
                </q-card-section>
              </div>
            </q-slide-transition>
          </q-card>
        </div>
      </div>
    </div>
  </q-page>
</template>

<script>
import { ref, computed } from 'vue';
import { api } from 'boot/axios';

export default {
  name: 'ApodPage',
  setup() {
    const startDate = ref('');
    const endDate = ref('');
    const searchTerm = ref('');
    const apodData = ref([]);
    const expanded = ref({});

    const fetchApodData = async () => {
      try {
        const response = await api.get('/apod', {
          params: {
            api_key: 'zN4wMdgG2BhW5spaWAHFeAnBvl3faDaDZc4DPqZY',
            start_date: startDate.value,
            end_date: endDate.value
          }
        });
        apodData.value = response.data;
        expanded.value = {};
      } catch (error) {
        console.error('Error al consultar APOD:', error);
      }
    };

    const toggleExpand = (date) => {
      expanded.value[date] = !expanded.value[date];
    };

    const filteredApodData = computed(() => {
      if (!searchTerm.value) {
        return apodData.value;
      }
      return apodData.value.filter(item =>
        item.explanation.toLowerCase().includes(searchTerm.value.toLowerCase())
      );
    });

    return {
      startDate,
      endDate,
      searchTerm,
      apodData,
      fetchApodData,
      filteredApodData,
      expanded,
      toggleExpand
    };
  }
};
</script>

<style lang="sass" scoped>
.bg-page
  background: url('https://i.pinimg.com/originals/9f/ea/c1/9feac1a4b56f22bdb2ca6926ff8c80db.jpg') no-repeat center center fixed
  background-size: cover

.filter-container
  position: fixed
  top: 50px
  left: 10px
  z-index: 1000

.content-container
  margin-left: 240px

.my-card
  width: 100%
  max-width: 350px
</style>
