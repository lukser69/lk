<template>
  <StatusCard class="order-card">{{ params.id }}</StatusCard>
</template>

<script setup lang="ts">
import { StatusCard } from "@/shared/ui/status-card";

const params: { id: string } = useRoute().params as { id: string };
</script>

<style lang="scss">
@import "~/shared/assets/styles/components/order/order";
</style>
