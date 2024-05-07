<template>
  <div class="orders-filters">
    <MainFilters />
    <YearFilter />
  </div>
</template>

<script setup lang="ts">
import MainFilters from "./main-filters/index.vue";
import YearFilter from "./year-filter/index.vue";
</script>

<style lang="scss">
@import "~/shared/assets/styles/components/orders-filters/orders-filters";
</style>
