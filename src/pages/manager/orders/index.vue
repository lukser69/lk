<template>
  <FixedLeftColumn>
    <template #fixed>
      <OrdersFilters />
    </template>
    <template #default>
      <Orders />
    </template>
  </FixedLeftColumn>
</template>

<script setup lang="ts">
import { FixedLeftColumn } from "@/shared/ui/templates";
import { OrdersFilters } from "@/widgets/orders-filters";
import { Orders } from "@/widgets/orders";
</script>
