<template>
  <div>
    <Button class="btn-back" color="purple" width="full" size="l">Назад</Button>

    <ul v-if="orders?.length" class="orders-list">
      <li v-for="order in orders" :key="order.id" class="orders-list__item">
        <StatusCard @click="goToOrder(order.id)">{{ order.id }}</StatusCard>
      </li>
    </ul>

    <Empty v-else>Заказов еще нет</Empty>
  </div>
</template>

<script setup lang="ts">
import { Button } from "@/shared/ui/button";
import { StatusCard } from "@/shared/ui/status-card";
import { Empty } from "@/shared/ui/empty";
import { internalAPIFetch } from "@/shared/api/internal/instance";

import type { IOrder } from "./types";

const router = useRouter();

const orders = ref<null | IOrder[]>(null);

const getOrders = async () => {
  try {
    interface IResponse {
      response: {
        data: {
          orders: IOrder[];
        };
      };
    }

    const response: IResponse = await internalAPIFetch("/orders.getTest");
    orders.value = response?.response?.data?.orders;
    // console.log(response);
  } catch (e) {
    console.error(e);
  }
};

onMounted(getOrders);

const goToOrder = (orderId: string) => {
  const currentPath = router.currentRoute.value.path;
  router.push(currentPath + `/${orderId}`);
};
</script>

<style lang="scss">
@import "~/shared/assets/styles/components/orders/orders-list";
</style>
