<script setup lang="ts">
import { onMounted, ref, nextTick } from 'vue'

const emits = defineEmits(['changeList'])
const sortableListRef = ref()
onMounted(async () => {
  await nextTick()
  const sortableList = document.querySelector('.sortable-list')
  if (sortableList) {
    const items = sortableList.querySelectorAll('.item')

    items.forEach((item: any) => {
      item.addEventListener('dragstart', () => {
        // Adding dragging class to item after a delay
        setTimeout(() => item.classList.add('dragging'), 0)
      })
      // Removing dragging class from item on dragend event
      item.addEventListener('dragend', () => {
        item.classList.remove('dragging')
        emits('changeList', [...sortableListRef.value.children])
      })
    })

    const initSortableList = (e: any) => {
      const draggingItem = document.querySelector('.dragging') as any
      // Getting all items except currently dragging and making array of them
      let siblings = [...sortableList.querySelectorAll('.item:not(.dragging)')]

      // Finding the sibling after which the dragging item should be placed
      let nextSibling: any = siblings.find((sibling: any) => {
        return e.clientY <= sibling.offsetTop + sibling.offsetHeight / 2
      })

      // Inserting the dragging item before the found sibling
      sortableList.insertBefore(draggingItem, nextSibling)
    }
    sortableList.addEventListener('dragover', initSortableList)
    sortableList.addEventListener('dragenter', (e: any) => e.preventDefault())
  }
})
</script>

<template>
  <div class="sortable-list" ref="sortableListRef">
    <slot></slot>
  </div>
</template>

<style>
.sortable-list {
  width: 425px;
  padding: 25px;
  background: #fff;
  border-radius: 7px;
  padding: 30px 25px 20px;
  box-shadow: 0 15px 30px rgba(0, 0, 0, 0.1);
}

.sortable-list .item {
  list-style: none;
  display: flex;
  cursor: move;
  background: #fff;
  align-items: center;
  border-radius: 5px;
  padding: 10px 13px;
  margin-bottom: 11px;
  border: 1px solid #ccc;
  justify-content: space-between;
  min-height: 47px;
}

.item .details {
  display: flex;
  align-items: center;
}

.item .details img {
  height: 43px;
  width: 43px;
  pointer-events: none;
  margin-right: 12px;
  object-fit: cover;
  border-radius: 50%;
}

.item .details span {
  font-size: 1.13rem;
}

.item i {
  color: #474747;
  font-size: 1.13rem;
}

.item.dragging {
  opacity: 0;
}

.item.dragging :where(.details, i) {
  opacity: 0;
}
</style>
