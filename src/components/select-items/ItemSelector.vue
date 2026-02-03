<script setup>
import Item from '@/components/select-items/Item.vue'
import { computed, ref } from 'vue'

const props = defineProps({
  items: {
    type: Object,
    required: true,
    validator(value, props) {
      if (!Array.isArray(value)) {
        return false;
      }

      for (const valueElement of value) {
        if (!valueElement?.id || !valueElement.name) {
          return false;
        }
      }

      return true;
    },
  },
  multiple: {
    type: Boolean,
    default: false,
  },

  floatRight: {
    type: Boolean,
    default: false,
  },
})

const itemsMaxLimit = computed(() => {
  return props.items.length >= 6
    ? 6
    : props.items.length
})

const selectedIds = ref([])

const selectedItems = computed(() => {
  return props.items.filter(item => selectedIds.value.includes(item.id))
})

const unselectedItems = computed(() => {
  return props.items.filter(item => !selectedIds.value.includes(item.id))
})

const selectItem = id => {
  if (props.multiple) {
    if (itemsMaxLimit.value > selectedIds.value.length) {
      selectedIds.value.push(id)
    } else {
      alert(`Вы не можете добавить больше ${itemsMaxLimit.value} элементов согласно условию задания!`)
    }
  } else {
    selectedIds.value = [id]
  }
}
const unselectItem = id => {
  if (props.multiple) {
    selectedIds.value = selectedIds.value.filter(itemId => itemId !== id)
  } else {
    selectedIds.value = []
  }
}
</script>

<template>
  <div class="item-selector" :class="{'float-right': floatRight}">
    <div class="item-selector__head">
      <div class="item-selector__head-wrap" :class="{'large': !multiple}">
        <item
          :key="i"
          v-for="(item, i) in selectedItems"
          :item
          :large="!multiple"
          @click="unselectItem(item.id)"
        />
      </div>

      <div class="item-selector__head-title" v-if="multiple">
        Элементов выбрано: {{ selectedIds.length }} из {{ itemsMaxLimit }}
      </div>
      <div class="item-selector__head-title" v-else>
        {{ selectedIds.length ? 'Выбран' : 'Не выбран' }}
      </div>
    </div>

    <div class="item-selector__body">
      <item
        :key="i"
        v-for="(item, i) in unselectedItems"
        :item
        @click="selectItem(item.id)"
      />
    </div>
  </div>
</template>

<style scoped lang="scss">
.item-selector {
  display: flex;
  flex-direction: column;
  gap: 50px;
  width: 100%;
  padding: 20px 0;

  &.float-right {
    align-items: flex-end;
  }

  & > * {
    border: 3px solid lightgray;
    border-radius: 5px;
  }

  &__head {
    min-height: 270px;
    width: 70%;
    display: flex;
    flex-direction: column;
    justify-content: space-between;

    &-wrap {
      display: flex;
      flex-wrap: wrap;
      gap: 20px;
      padding: 20px;

      &.large {
        justify-content: center;
      }
    }

    &-title {
      text-align: center;
      padding-bottom: 10px;
    }
  }

  &__body {
    min-height: 300px;
    width: 100%;

    display: flex;
    flex-wrap: wrap;
    gap: 20px;
    align-items: flex-start;
    justify-content: flex-start;
    align-content: flex-start;
    padding: 20px;
  }
}
</style>
