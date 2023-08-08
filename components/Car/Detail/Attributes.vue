<template>
  <div class="mr-10 mt-5 border-b pb-5">
    <div class="flex text-lg mt-2" v-for="feat in features" :key="feat">
      <p class="rounded text-lime-800 mr-3">
        <span class="material-symbols-outlined"> check </span>
      </p>
      <p>{{ feat }}</p>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({ features: Array });
</script>

<style lang="scss" scoped></style>
