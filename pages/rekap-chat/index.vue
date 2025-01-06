<template>
    <div>
      <div class="text-fuchsia-500">{{ data }}</div>
    </div>  
  </template>
  
  <script lang="ts" setup>
    const client = useSanctumClient()
    const { data, error, refresh } = await useAsyncData('rekap-chat', fetchData)
    function fetchData() {
      return client(`/api/rekap-chat`);
    }
  </script>
  
  