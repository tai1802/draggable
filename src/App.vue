<script setup lang="ts">
import { onMounted, ref } from 'vue'
import Draggable from './components/Draggable/index.vue'
import DraggableItem from './components/Draggable/Item.vue'
import DraggableTable from './components/DraggableTable/index.vue'

const list = ref<any[]>([
  {
    name: 'Kristina Zasiadko',
    img: 'images/img-1.jpg'
  },
  {
    name: 'Gabriel Wilson',
    img: 'images/img-2.jpg'
  },
  {
    name: 'Ronelle Cesicon',
    img: 'images/img-3.jpg'
  },
  {
    name: 'James Khosravi',
    img: 'images/img-4.jpg'
  },
  {
    name: 'Annika Hayden',
    img: 'images/img-5.jpg'
  },
  {
    name: 'Donald Horton',
    img: 'images/img-6.jpg'
  }
])

onMounted(() => {
  if (localStorage.getItem('data')) {
    list.value = JSON.parse(localStorage.getItem('data') || '') || []
  } else {
    localStorage.setItem('data', JSON.stringify(list.value))
  }
})

const changeList = (value: any) => {
  localStorage.setItem(
    'data',
    JSON.stringify(
      value.map((e: any) => {
        return {
          name: e.innerText,
          img: String(e.children[0].children[0].src).replace(document.location.origin, '')
        }
      })
    )
  )
}
const active = ref<number>(-1)
</script>

<template>
  <div>
    <!-- <Draggable @change-list="changeList">
      <DraggableItem
        v-for="(item, idx) in list"
        :key="item"
        :draggable="active === idx"
        style="cursor: default"
      >
        <div class="details">
          <img :src="item.img" />
          <span>{{ item.name }}</span>
        </div>
        <i
          class="uil uil-draggabledots"
          style="cursor: move"
          @mousemove="active = idx"
          @mouseleave="active = -1"
        ></i>
      </DraggableItem>
    </Draggable> -->
  </div>
  <DraggableTable />
</template>

<style scoped></style>
