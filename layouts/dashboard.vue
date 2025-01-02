<template>
  <div class="dashboard">
    <main class="bg-zinc-200 dark:bg-zinc-950">
      <div class="flex">
        <SidebarMenu />
        <div class="w-full p-4 grow">
          <Header />
          <div class="bg-zinc-50 dark:bg-zinc-900 rounded-lg p-4 mb-4 mt-4">
            <div class="mx-auto">
              <h1 class="text-2xl font-bold mb-4">{{ $route.meta.title }}</h1>
              <slot />
            </div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>