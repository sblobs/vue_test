<script setup>
import { ref, computed, useTemplateRef, onMounted } from 'vue'

const numList = ref([
  "1",
  "2",
  "3",
  "4",
  "5",
  "6",
  "7",
  "8",
  "9",
  "10",
  "11",
  "12",
  "13",
  "14",
  "15"
])
const virtualizedList = ref([])
const scrollContainerRef = useTemplateRef('scrollContainer')
const itemHeight = 24
const safeRow = 1

onMounted(() => {
  handleScroll(0)
})

const calcContainerHeight = () => {
  const itemCount = numList.value.length
  return Math.ceil(itemCount) * (itemHeight)
}

const virtualContainerStyle = computed(() => {
  return {
    position: 'relative',
    height: `${calcContainerHeight()}px`
  }
})

const handleScroll = (visibleTop) => {
  const visibleBottom = visibleTop + scrollContainerRef.value.clientHeight
  const startIndex = Math.max((Math.floor(visibleTop / itemHeight) - safeRow), 0)
  const endIndex = Math.ceil(visibleBottom / itemHeight) + safeRow
  console.log('startIndex', startIndex, 'endIndex', endIndex, 'visibleTop', visibleTop, 'visibleBottom', visibleBottom)
  virtualizedList.value = numList.value.slice(startIndex, endIndex).map((item, index) => {
    return {
      number: item,
      style: {
        position: 'absolute',
        top: `${Math.floor(index + startIndex) * itemHeight}px`
      }
    }
  })
  console.log("virtualized list is", virtualizedList)
}

</script>

<template>
    <div
      class="list-container"
      @scroll="(event) => handleScroll(event.currentTarget.scrollTop)"
      ref="scrollContainer"
    >
      <div
        class="virtual-list-container"
        :style="virtualContainerStyle"
      >
        <div
          v-for="item in virtualizedList"
          :style="item.style"
        >
          {{ item.number }}
        </div>
      </div>
    </div>
</template>

<style scoped>

.list-container {
  height: 90px;
  overflow: auto;
  border-style: solid;
}

</style>
