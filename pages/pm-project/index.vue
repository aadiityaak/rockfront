<template>
    <div>
      <div class="text-blue-500">{{ data }}</div>
    </div>  
  </template>
  
  <script lang="ts" setup>
    const client = useSanctumClient()
    const { data, error, refresh } = await useAsyncData('pm-project', fetchData)
    function fetchData() {
      return client(`/api/pm-project`);
    }
  </script>
  
  