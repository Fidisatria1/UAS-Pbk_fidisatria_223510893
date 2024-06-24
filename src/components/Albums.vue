<template>
  <q-page class="albums-container">
    <h1 class="page-title">Albums</h1>
    <p class="page-description">Pilih atau klik daftar albums di bawah ini untuk melihat foto-foto yang ada.</p>
    <q-list bordered v-if="!loading">
      <q-item v-for="album in albums" :key="album.id" clickable @click="goToAlbum(album.id)" class="hover-item">
        <q-item-section>
          <q-item-label>{{ album.title }}</q-item-label>
        </q-item-section>
      </q-item>
    </q-list>
    <q-spinner v-if="loading" color="primary" />
  </q-page>
</template>

<script>
import { ref, onMounted } from 'vue';
import { useRouter } from 'vue-router';

export default {
  name: 'Albums',
  setup() {
    const router = useRouter();
    const albums = ref([]);
    const loading = ref(true);

    const fetchAlbums = async () => {
      const response = await fetch('https://jsonplaceholder.typicode.com/albums');
      albums.value = await response.json();
      loading.value = false;
    };

    onMounted(fetchAlbums);

    const goToAlbum = (id) => {
      loading.value = true;
      router.push(`/albums/${id}`);
    };

    return {
      albums,
      goToAlbum,
      loading,
    };
  },
};
</script>

<style scoped>
.albums-container {
  max-width: 800px;
  margin: 20px auto;
  padding: 0 20px;
}

.page-title {
  font-size: 2rem;
  font-weight: 700;
  text-align: center;
  margin-bottom: 20px;
  color: #333;
  margin-bottom: 10px;
}

.page-description {
  font-size: 16px;
  margin-bottom: 20px;
}

.hover-item {
  transition: all 0.3s ease;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.hover-item:hover {
  background-color: #f0f0f0;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.q-item-label {
  font-size: 18px;
}

@media (max-width: 600px) {
  .albums-container {
    padding: 0 10px;
  }

  .page-title {
    font-size: 20px;
  }

  .page-description {
    font-size: 14px;
  }
}
</style>
