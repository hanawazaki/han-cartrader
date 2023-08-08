<template>
  <div class="shadow border w-64 mr-10 z-30 h-[190px]">
    <!-- location start -->
    <div class="p-5 flex justify-between relative cursor-pointer border-b">
      <h3>Location</h3>
      <h3 class="text-blue-400 capitalize" @click="updateModal('location')">
        {{ route.params.city }}
      </h3>
      <div
        class="absolute border shadow left-56 p-5 top-1 -m-1 bg-white"
        v-if="modal.location"
      >
        <input type="text" class="border p-1 rounded" v-model="city" />
        <button
          @click="onChangeLocation"
          class="bg-blue-400 w-full mt-2 rounded text-white p-1"
        >
          Apply
        </button>
      </div>
    </div>
    <!-- location end -->

    <!-- make start -->
    <div class="p-5 flex justify-between relative cursor-pointer border-b">
      <h3>Make</h3>
      <h3 class="text-blue-400 capitalize" @click="updateModal('make')">
        {{ route.params.make || "Any" }}
      </h3>
      <div
        v-if="modal.make"
        class="absolute border shadow left-56 p-5 top-1 -m-1 w-[600px] flex justify-between flex-wrap bg-white"
      >
        <h4
          v-for="make in makes"
          :key="make"
          class="w-1/3"
          @click="onChangeMake(make)"
        >
          {{ make }}
        </h4>
      </div>
    </div>
    <!-- make end -->

    <!-- price start -->
    <div class="p-5 flex justify-between relative cursor-pointer border-b">
      <h3>Price</h3>
      <h3 class="text-blue-400 capitalize" @click="updateModal('price')">
        {{ priceRangeText }}
      </h3>
      <div
        class="absolute border shadow left-56 p-5 top-1 -m-1 bg-white"
        v-if="modal.price"
      >
        <input
          type="number"
          class="border p-1 rounded"
          placeholder="Min"
          v-model="priceRange.min"
        />
        <input
          type="number"
          class="border p-1 rounded"
          placeholder="Max"
          v-model="priceRange.max"
        />

        <button
          class="bg-blue-400 w-full mt-2 rounded text-white p-1"
          @click="onChangePrice"
        >
          Apply
        </button>
      </div>
    </div>
    <!-- price end -->
  </div>
</template>

<script setup>
const { makes } = useCars();
const priceRange = ref({
  min: "",
  max: "",
});
const modal = ref({
  make: false,
  location: false,
  price: false,
});
const city = ref("");
const route = useRoute();
const router = useRouter();
// console.log(route.query);
const priceRangeText = computed(() => {
  const minPrice = route.query.minPrice;
  const maxPrice = route.query.maxPrice;

  if (!minPrice && !maxPrice) return "Any";
  else if (!minPrice && maxPrice) {
    return `< $${maxPrice}`;
  } else if (minPrice && !maxPrice) {
    return `> $${minPrice}`;
  } else {
    return `> $${minPrice}-$${maxPrice}`;
  }
});
const updateModal = (key) => {
  modal.value[key] = !modal.value[key];
};

const onChangeMake = (make) => {
  updateModal("make");
  navigateTo(`/city/${route.params.city}/car/${make}`);
};

const onChangeLocation = () => {
  if (!city.value) return;
  if (!isNaN(parseInt(city.value))) {
    throw createError({
      statusCode: 400,
      message: "Invalid city format",
    });
  }
  updateModal("location");
  navigateTo(`/city/${city.value}/car/${route.params.make}`);
};

const onChangePrice = () => {
  updateModal("price");
  if (priceRange.value.max && priceRange.value.min) {
    if (priceRange.value.min > priceRange.value.max) return;
  }

  router.push({
    query: {
      minPrice: priceRange.value.min,
      maxPrice: priceRange.value.max,
    },
  });
};
</script>

<style lang="scss" scoped></style>
