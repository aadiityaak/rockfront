<template>
    <div>
      <div class="text-fuchsia-500">{{ data }}</div>
    </div>  
  </template>
  
  <script lang="ts" setup>
    const client = useSanctumClient()
    const { data, error, refresh } = await useAsyncData('quality', fetchData)
    function fetchData() {
      return client(`/api/quality`);
    }
  </script>
  
  