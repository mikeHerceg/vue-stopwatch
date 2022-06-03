<script setup lang="ts">
  const props = defineProps({
    onClick: Function,
    disabled: Boolean
  })
</script>

<template>
  <button @click="onClick" :disabled="disabled" >
      <slot></slot>
  </button>
</template>

<style lang="scss" scoped>
  
</style>