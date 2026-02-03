<script setup>
const props = defineProps({
  item: {
    type: Object,
    required: true,
    validator(value, props) {
      return !(!value?.id || !value.name);
    },
  },
  large: {
    type: Boolean,
    default: false,
  },
})

</script>

<template>
  <div class="item" :class="{'large': large}">
    {{ item.name }}
  </div>
</template>

<style scoped>
.item {
  padding: 10px 5px;
  color: lightgray;
  border: 2px solid lightgray;
  border-radius: 5px;
  min-height: 80px;
  min-width: 80px;
  cursor: pointer;

  &.large {
    min-height: 160px;
    min-width: 160px;
  }

  &:hover {
    background: #D3D3D351;
    color: darkgray;
    border: 2px solid darkgray;
  }
}
</style>
