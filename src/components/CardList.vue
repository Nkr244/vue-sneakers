<script setup>
import CardComponent from '@/components/CardComponent.vue'

defineProps({
  items: Array,
  isFavorites: Boolean
})

const emit = defineEmits(['addToFavorite', 'addToCart'])

</script>

<template>
  <div class="grid grid-cols-4 gap-5" v-auto-animate>
    <CardComponent
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :imageUrl="item.imageUrl"
      :title="item.title"
      :price="item.price"
      :isAdded="item.isAdded"
      :isFavorite="item.isFavorite"
      :onClickAdd="isFavorites ? null : () =>   emit('addToCart', item)"
      :onClickFavorite="isFavorites ? null : () => emit('addToFavorite', item)"
    />
  </div>
</template>
