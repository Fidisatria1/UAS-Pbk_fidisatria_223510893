<template>
  <q-page class="photo-detail-container">
    <q-card class="hover-card gray-card">
      <q-card-section v-if="!loading">
        <img :src="photo.url" :alt="photo.title" />
        <div class="photo-info">
          <q-item-label>{{ photo.title }}</q-item-label>
        </div>
      </q-card-section>
      <q-spinner v-if="loading" color="primary" />
    </q-card>
  </q-page>
</template>

<script>
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';

export default {
  name: 'PhotoDetail',
  setup() {
    const route = useRoute();
    const photo = ref({});
    const loading = ref(true);

    const fetchPhoto = async () => {
      const response = await fetch(`https://jsonplaceholder.typicode.com/photos/${route.params.id}`);
      photo.value = await response.json();
      loading.value = false;
    };

    onMounted(fetchPhoto);

    return {
      photo,
      loading
    };
  }
};
</script>

<style scoped>
.photo-detail-container {
  max-width: 800px;
  margin: 20px auto;
  padding: 0 20px;
}

img {
  width: 100%;
  height: auto;
  margin-bottom: 20px;
}

.photo-info {
  text-align: center;
}

.q-item-label {
  font-size: 18px;
}

.hover-card {
  transition: all 0.3s ease;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.hover-card:hover {
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.gray-card {
  background-color: #f0f0f0; /* Light gray background color */
}

@media (max-width: 600px) {
  .photo-detail-container {
    padding: 0 10px;
  }
}
</style>