<script setup>
import { RouterView, RouterLink } from 'vue-router';
import HomeView from './views/HomeView.vue';

</script>

<template>
  <main>
    
    <!-- <RouterView /> -->

    <Suspense>
      <template #default>
         <!--template yang akan menampilkan isi konten/component yang akan dimuat  -->
        <HomeView />
      </template>

      <template #fallback>
        <!-- template yang akan dipanggil terlebih dahulu sebelum component dimuat -->
        <p>Loading ...</p>
      </template>
    </Suspense>
  </main>
</template>

<style scoped>

</style>
