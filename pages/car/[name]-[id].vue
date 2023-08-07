<template>
  <div>
    <CarDetailHero />
  </div>
  <CarDetailAttributes />
  <CarDetailDescription />
  <CarDetailContact />
</template>

<script setup>
const { toTitleCase } = useUtilities();

const route = useRoute();
definePageMeta({
  layout: "custom",
});

useHead({
  title: toTitleCase(route.params.name),
});
</script>

<style lang="scss" scoped></style>
