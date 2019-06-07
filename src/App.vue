<template>
  <div id="app" class="bg-grey-100">
    <div id="nav">
      <div class="brand"></div>

      <router-link to="/">Force Directed Graph</router-link>
    </div>
    <router-view />
  </div>
</template>

<style src="./assets/app.css"></style>
<style lang="postcss" scoped>
#nav {
  @apply bg-blue-900 h-12 flex justify-between items-center text-white px-4;
}
</style>
