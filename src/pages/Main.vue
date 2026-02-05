<script setup>
import ItemSelector from '@/components/select-items/ItemSelector.vue'
import { computed } from 'vue'
import { getItems } from '@/api/loadItemsService.js'

const userItems = computed(() => {
  return getItems(true)
})

const toChoseItems = computed(() => {
  return getItems(false)
})

</script>

<template>
  <div class="main">
    <h1>Тестовое задание для вакансии</h1>

    <div class="main__body">
      <item-selector :items="userItems" multiple />
      <item-selector :items="toChoseItems" float-right />
    </div>
  </div>
</template>

<style scoped lang="scss">
.main {
  padding: 10px 20px;

  h1 {
    text-align: center;
  }

  &__body {
    display: flex;
    justify-content: space-between;
    gap: 20px;
  }
}
</style>
