<!-- eslint-disable vue/multi-word-component-names -->
<script setup lang="ts">
import { computed } from 'vue'
import type { Order } from '@/types/Order'
import OrderStatus from '@/components/atoms/OrderStatus.vue'

const props = defineProps<{ order: Order }>()

const totalPrice = computed(() =>
  props.order.Products?.reduce(
    (total, item) => total + (item.price || 0) * (item.OrderProduct?.quantity || 0),
    0
  )
)
</script>
<template>
  <div class="flex flex-wrap items-center gap-y-4 py-6">
    <dl class="w-1/2 sm:w-1/4 lg:w-auto lg:flex-1">
      <dt class="text-base font-medium text-gray-500">Order ID:</dt>
      <dd class="mt-1.5 text-base font-semibold text-gray-900">
        <a href="#" class="hover:underline">#{{ order.id }}</a>
      </dd>
    </dl>

    <dl class="w-1/2 sm:w-1/4 lg:w-auto lg:flex-1">
      <dt class="text-base font-medium text-gray-500">Date:</dt>
      <dd class="mt-1.5 text-base font-semibold text-gray-900">
        {{ new Date(order.order_date).toISOString().split('T')[0] }}
      </dd>
    </dl>

    <dl class="w-1/2 sm:w-1/4 lg:w-auto lg:flex-1">
      <dt class="text-base font-medium text-gray-500">Price:</dt>
      <dd class="mt-1.5 text-base font-semibold text-gray-900">${{ totalPrice }}</dd>
    </dl>

    <dl class="w-1/2 sm:w-1/4 lg:w-auto lg:flex-1">
      <dt class="text-base font-medium text-gray-500">Status:</dt>
      <dd
        class="me-2 mt-1.5 inline-flex items-center rounded bg-primary-100 px-2.5 py-0.5 text-xs font-medium text-primary-800"
      >
        <OrderStatus :status="order.status" />
      </dd>
    </dl>

    <div class="w-full grid sm:grid-cols-2 lg:flex lg:w-64 lg:items-center lg:justify-end gap-4">
      <!-- <button
        v-if="order.status === 'Pending'"
        type="button"
        class="w-full rounded-lg border border-red-700 px-3 py-2 text-center text-sm font-medium text-red-700 hover:bg-red-700 hover:text-white focus:outline-none focus:ring-4 focus:ring-red-300 lg:w-auto"
      >
        Cancel order
      </button> -->
      <RouterLink
        :to="'/orders/' + order.id"
        class="w-full inline-flex justify-center rounded-lg border border-gray-200 bg-white px-3 py-2 text-sm font-medium text-gray-900 hover:bg-gray-100 hover:text-primary-700 focus:z-10 focus:outline-none focus:ring-4 focus:ring-gray-100 lg:w-auto"
        >View details</RouterLink
      >
    </div>
  </div>
</template>
