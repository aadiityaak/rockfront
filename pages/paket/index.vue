<template>
  <div>
    <h1 class="text-2xl font-bold mb-4">Paket</h1>
    <div class="text-red-500">{{ data }}</div>
  </div>  
</template>

<script lang="ts" setup>
  const client = useSanctumClient()
  const { data, error, refresh } = await useAsyncData('paket', fetchData)
  function fetchData() {
    return client(`/api/paket`);
  }
</script>

