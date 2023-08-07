<template>
  <div>
    <div class="mt-32 flex">
      <CarSideBar />
      <NuxtPage />
    </div>
  </div>
</template>

<script setup>
const route = useRoute();
const { toTitleCase } = useUtilities();
definePageMeta({
  layout: "custom",
});

useHead({
  title: `${
    route.params.make ? toTitleCase(route.params.make) : "Cars"
  } in ${toTitleCase(route.params.city)}`,
});
</script>

<style lang="scss" scoped></style>
