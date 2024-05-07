<template>
  <div class="block orders-year-filter">
    <Select v-model="selectedYear" :items="yearsItems" label="Год" />

    <div class="orders-year-filter__actions">
      <Button size="l" @click="setOrdersFilterYear(selectedYear)">Показать</Button>
      <Button size="l" color="gray-outline" @click="removeOrderFilterYear">Сбросить</Button>
    </div>

    <div class="orders-year-filter__hint">
      Обычный вывод показывает 250 заказов, чтобы снять ограничение и показать до 5000 заказов нужно выбрать год.
    </div>
  </div>
</template>

<script setup lang="ts">
import { Select } from "@/shared/ui/select";
import { Button } from "@/shared/ui/button";
import type { SelectItems } from "@/shared/ui/select";

const router = useRouter();

const selectedYear = ref<string>();

const yearsItems: SelectItems = [
  { title: "2024", value: "2024" },
  { title: "2023", value: "2023" },
  { title: "2022", value: "2022" },
  { title: "2021", value: "2021" }
];

const setOrdersFilterYear = (year: string | undefined) => {
  if (!year) {
    removeOrderFilterYear();
    return;
  }

  const currentParams = { ...router.currentRoute.value.query };
  currentParams.year = year;
  router.push({
    query: currentParams
  });
};

const removeOrderFilterYear = () => {
  selectedYear.value = "";

  const currentParams = { ...router.currentRoute.value.query };

  delete currentParams.year;

  router.push({
    query: currentParams
  });
};
</script>

<style lang="scss">
@import "~/shared/assets/styles/components/orders-filters/orders-year-filter";
</style>
