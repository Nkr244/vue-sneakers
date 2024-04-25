<script setup>
import { onMounted, ref } from 'vue'
import axios from 'axios'
import CardList from '@/components/CardList.vue'

const favorites = ref([])

onMounted(async () => {
  try {
    const { data } = await axios.get('https://239ace64ca8c2e33.mokky.dev/favorites?_relations=items')

    favorites.value = data.map(obj => obj.item)

  } catch (err) {
    console.log(err)
  }
})

</script>

<template>
  <h1 class="mb-8 text-3xl"><b>Мои закладки</b></h1>

  <CardList :items="favorites" is-favorites />
</template>