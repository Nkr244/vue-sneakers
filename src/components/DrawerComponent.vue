<script setup>
import DrawerHead from '@/components/DrawerHead.vue'
import CartListItem from '@/components/CartListItem.vue'
import InfoBlock from '@/components/InfoBlock.vue'
import axios from 'axios'
import { ref, provide, computed, inject } from 'vue'

const props = defineProps({
  totalPrice: Number,
  vatPrice: Number
})


const { cart, closeDrawer } = inject('cart')

const isCreatingOrder = ref(false)
const orderId = ref(null)

const createOrder = async () => {
  try {
    isCreatingOrder.value = true
    const { data } = await axios.post(`https://239ace64ca8c2e33.mokky.dev/orders`, {
      items: cart.value,
      totalPrice: props.totalPrice.value
    })

    cart.value = []

    orderId.value = data.id
  } catch (err) {
    console.log(err)
  } finally {
    isCreatingOrder.value = false
  }
}
const cartIsEmpty = computed(() => cart.value.length === 0)
const cartButtonDisabled = computed(
  () => isCreatingOrder.value || cartIsEmpty.value)


</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
    <DrawerHead />

    <div v-if="!totalPrice || orderId" class="flex h-full items-center justify-center">
      <InfoBlock
        v-if="!totalPrice && !orderId"
        title="Корзина пуста"
        description="Добавьте что-то..."
        image-url="/package-icon.png" />

      <InfoBlock
        v-if="orderId"
        title="Заказ оформлен"
        :description="`Ваш заказ #${orderId}`"
        image-url="/order-success-icon.png" />
    </div>


    <div v-else>
      <CartListItem v-if="totalPrice" />

      <div v-if="totalPrice" class="flex flex-col gap-3 mb-5 my-7">
        <div class="flex gap-2">
          <span>Итого:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ totalPrice }} Р</b>
        </div>
        <div class="flex gap-2">
          <span>Налог:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ vatPrice }} Р</b>
        </div>
        <button
          :disabled="cartButtonDisabled"
          @click="createOrder"
          class=" mt-4 bg-lime-400 w-full rounded-xl
             py-3 text-white hover:bg-lime-600
             transition active:bg-lime-700
             disabled:bg-slate-300
             cursor-pointer">
          Оформить заказ
        </button>
      </div>
    </div>


  </div>
</template>