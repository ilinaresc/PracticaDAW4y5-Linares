<template>
  <q-card class="apod-card" flat bordered>
    <template v-if="item.media_type === 'image'">
      <q-img :src="item.url" class="apod-media">
        <div class="absolute-bottom text-subtitle2 text-center">
          {{ item.title }}
        </div>
      </q-img>
    </template>
    <template v-else>
      <q-video :src="item.url" class="apod-media" />
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
        :icon="expanded ? 'keyboard_arrow_up' : 'keyboard_arrow_down'"
        @click="$emit('toggleExpand', item.date)"
      />
    </q-card-actions>
    <q-slide-transition>
      <div v-show="expanded">
        <q-separator />
        <q-card-section class="text-subtitle2">
          {{ item.explanation }}
        </q-card-section>
      </div>
    </q-slide-transition>
  </q-card>
</template>

<script>
export default {
  name: 'ApodItem',
  props: {
    item: Object,
    expanded: Boolean
  }
};
</script>

<style scoped>
.apod-card {
  width: 100%;
  max-width: 350px;
}

.apod-media {
  width: 100%;
  height: 200px;
  object-fit: cover;
}
</style>
