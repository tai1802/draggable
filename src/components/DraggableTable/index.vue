<script setup lang="ts">
import { onMounted, ref, nextTick } from 'vue'

const emits = defineEmits(['changeList'])
const sortableListRef = ref()
onMounted(async () => {
  await nextTick()
  const sortableList = document.querySelector('.tbody-draggable')
  if (sortableList) {
    const items = sortableList.querySelectorAll('.tr-item')

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
      let siblings = [...sortableList.querySelectorAll('.tr-item:not(.dragging)')]

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
  <div>
    <table>
      <thead>
        <th></th>
        <th></th>
        <th></th>
      </thead>
      <tbody class="tbody-draggable" ref="sortableListRef">
        <tr draggable="true" class="tr-item">
          <td>A1</td>
          <td>B1</td>
          <td>C1</td>
        </tr>
        <tr draggable="true" class="tr-item">
          <td>A2</td>
          <td>B2</td>
          <td>C2</td>
        </tr>
        <tr draggable="true" class="tr-item">
          <td>A3</td>
          <td>B3</td>
          <td>C3</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style>
table {
  width: 425px;
  border: 1px solid #474747;
}

thead th:nth-child(1) {
  width: 30%;
}

thead th:nth-child(2) {
  width: 20%;
}

thead th:nth-child(3) {
  width: 15%;
}

thead th:nth-child(4) {
  width: 35%;
}

th,
td {
  padding: 20px;
}

thead,
tfoot {
  background: url(leopardskin.jpg);
  color: white;
  text-shadow: 1px 1px 1px black;
}

td {
  background: linear-gradient(to bottom, rgba(0, 0, 0, 0.1), rgba(0, 0, 0, 0.5));
  border: 1px solid black;
}

.tbody-draggable .tr-item {
  cursor: move;
}

.tr-item.dragging {
  opacity: 0;
}

.tr-item.dragging :where(.details, i) {
  opacity: 0;
}
</style>
