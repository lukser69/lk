<template>
  <div class="block orders-main-filters">
    <InputDate
      v-model="searchDatePhotoshoot"
      :range="true"
      placeholder="Период"
      @update:model-value="val => setDateOrderFilters(val)"
    />
    <Search
      v-model:search-query="searchValue"
      v-model:search-type="searchType"
      :search-types="searchTypes"
      select-display-value="icon"
      @submit="setTypeOrdersFilters"
      @clear="clearTypeOrderFilter"
    />

    <ToggleGroup class="orders-main-filters__sorting-list">
      <Toggle :pressed="isActiveAllSortingItems()" color="gray" @click="toggleActiveAllSortingItem">Все</Toggle>

      <Toggle
        v-for="item in sortingListItems"
        :key="item.id"
        :pressed="isActiveSortingItems(item.id)"
        color="gray"
        @click="toggleSortingItem(item.id)"
      >
        <i class="fa-regular" :class="`fa-${item.name}`" />
      </Toggle>
    </ToggleGroup>
  </div>
</template>

<script setup lang="ts">
import type { ModelValue } from "@vuepic/vue-datepicker";

import { InputDate } from "@/shared/ui/inputs/input-date";
import { Search } from "@/shared/ui/search";
import { ToggleGroup } from "@/shared/ui/toggle-group";
import { Toggle } from "@/shared/ui/toggle";

import type { ISortingItem } from "./types";

const router = useRouter();

const searchTypes = {
  order_number: { title: "Номер заказа", placeholder: "Номер заказа" },
  psid: { title: "Номер фотосессии", placeholder: "Номер фотосессии" },
  client_id: { title: "Клиент ID", placeholder: "Клиент ID" },
  phone: { title: "Телефон", placeholder: "Телефон" },
  email: { title: "Email", placeholder: "Email" },
  payer: { title: "Плательщик, ребенок", placeholder: "Плательщик, ребенок" }
};

const searchDatePhotoshoot = ref();
const searchType = ref<string>(Object.keys(searchTypes)[0]);
const searchValue = ref<string>("");

const setTypeOrdersFilters = () => {
  const currentParams = { ...router.currentRoute.value.query };

  if (searchValue.value) {
    currentParams.search_type = searchType.value;
    currentParams.search_value = searchValue.value;

    router.push({
      query: currentParams
    });
  } else {
    clearTypeOrderFilter();
  }
};

const clearTypeOrderFilter = () => {
  searchValue.value = "";

  const currentParams = { ...router.currentRoute.value.query };

  delete currentParams.search_type;
  delete currentParams.search_value;

  router.push({
    query: currentParams
  });
};

const setDateOrderFilters = (dates: ModelValue & Date[]) => {
  const currentParams = { ...router.currentRoute.value.query };

  if (dates[0]) {
    currentParams.date_start = editDate(dates[0].toLocaleDateString("ru-RU"));
  } else {
    delete currentParams.date_start;
  }

  if (dates[1]) {
    currentParams.date_finish = editDate(dates[1].toLocaleDateString("ru-RU"));
  } else {
    delete currentParams.date_finish;
  }

  router.push({
    query: currentParams
  });
};

const editDate = (date: string) => {
  return date.split(".").reverse().join("");
};

const sortingListItems: ISortingItem[] = [
  { id: 1, name: "crown" },
  { id: 2, name: "thumbs-down" },
  { id: 3, name: "thumbs-up" },
  { id: 4, name: "suitcase" },
  { id: 5, name: "cloud-arrow-up" },
  { id: 6, name: "truck" },
  { id: 7, name: "money-bill-1" },
  { id: 8, name: "ruble-sign" }
];

const activeSortingItems = ref<number[]>([]);

const toggleSortingItem = (itemId: number) => {
  const foundItemId = activeSortingItems.value.findIndex(item => item === itemId);

  if (foundItemId === -1) {
    activeSortingItems.value.push(itemId);
  } else {
    activeSortingItems.value.splice(foundItemId, 1);
  }
};

const toggleActiveAllSortingItem = () => {
  if (activeSortingItems.value.length) {
    activeSortingItems.value.length = 0;
    return false;
  }

  sortingListItems.forEach(item => {
    activeSortingItems.value.push(item.id);
  });
};

const isActiveSortingItems = (itemId: number) => {
  return activeSortingItems.value.includes(itemId);
};

const isActiveAllSortingItems = () => {
  return activeSortingItems.value.length === sortingListItems.length;
};
</script>

<style lang="scss">
@import "~/shared/assets/styles/components/orders-filters/orders-main-filters";
</style>
