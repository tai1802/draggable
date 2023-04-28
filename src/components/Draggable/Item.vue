<script setup lang="ts">
import { toRef } from 'vue'

interface IProps {
  draggable?: boolean
}
const props = withDefaults(defineProps<IProps>(), {
  draggable: true
})
const draggable = toRef(props, 'draggable')
</script>

<template>
  <div :draggable="draggable" class="item">
    <slot></slot>
  </div>
</template>
