<template>
  <div class="default">
    <div class="flex flex-col min-h-screen justify-between">
      <hr class="border-zinc-200 dark:border-zinc-800" />
      <div class="grow bg-zinc-200 dark:bg-zinc-950 flex flex-col justify-center">
        <div class="flex flex-col justify-center">
          <slot />
        </div>
      </div>
    </div>
  </div>
</template>
