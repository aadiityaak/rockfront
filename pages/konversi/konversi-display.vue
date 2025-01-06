<template>
    <div>
      <div class="text-red-500">{{ data }}</div>
    </div>  
  </template>
  
  <script lang="ts" setup>
    const client = useSanctumClient()
    const { data, error, refresh } = await useAsyncData('konversi-display', fetchData)
    function fetchData() {
      return client(`/api/konversi-display`);
    }
  </script>
  
  