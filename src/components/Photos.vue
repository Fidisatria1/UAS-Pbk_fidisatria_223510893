<template>
  <q-page class="photos-container">
    <h1 class="page-title">Photos from Album: {{ albumTitle }}</h1>
    <q-list bordered v-if="!loading">
      <q-item v-for="photo in photos" :key="photo.id" clickable @click="viewPhoto(photo.id)" class="hover-item">
        <q-item-section>
          <img :src="photo.thumbnailUrl" :alt="photo.title" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ photo.title }}</q-item-label>
        </q-item-section>
      </q-item>
    </q-list>
    <q-spinner v-if="loading" color="primary" />
  </q-page>
</template>

<script>
import { ref, onMounted } from 'vue';
import { useRoute, useRouter } from 'vue-router';

export default {
  name: 'Photos',
  setup() {
    const route = useRoute();
    const router = useRouter();
    const photos = ref([]);
    const albumTitle = ref('');
    const loading = ref(true);

    const fetchPhotos = async () => {
      const response = await fetch(`https://jsonplaceholder.typicode.com/photos?albumId=${route.params.id}`);
      photos.value = await response.json();
      
      const albumResponse = await fetch(`https://jsonplaceholder.typicode.com/albums/${route.params.id}`);
      const album = await albumResponse.json();
      albumTitle.value = album.title;
      loading.value = false;
    };

    onMounted(fetchPhotos);

    const viewPhoto = (photoId) => {
      loading.value = true;
      router.push(`/photos/${photoId}`);
    };

    return {
      photos,
      albumTitle,
      viewPhoto,
      loading,
    };
  },
};
</script>

<style scoped>
.photos-container {
  max-width: 800px;
  margin: 20px auto;
  padding: 0 20px;
}

.page-title {
  font-size: 24px;
  margin-bottom: 20px;
}

.hover-item {
  transition: all 0.3s ease;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.hover-item:hover {
  background-color: #d3d3d3; /* Change to gray */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

img {
  width: 100px;
  height: 100px;
  cursor: pointer;
  margin-right: 20px;
}

.q-item-label {
  font-size: 18px;
}

@media (max-width: 600px) {
  .photos-container {
    padding: 0 10px;
  }

  .page-title {
    font-size: 20px;
  }

  img {
    width: 80px;
    height: 80px;
  }
}
</style>
