<!-- 추후 app.ts에 Start와 Set 통합 예정. -->
<template>
  <div
  @mousedown="logClickDown"
  @mousemove="mouseMoving"
  @mouseup="logClickUp"
  id="app-container">
    <div id="app-drag-box" :style="dragBoxStyle"></div>
  </div>

  <div id="app-bottom-container">
    <button @click="startRuler">Start ruler</button>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { useRoute } from 'vue-router'
import router from '@/router'

const route = useRoute()
const length = parseFloat(route.query.length?.toString() ?? "0")

let mouseDown = false
let startCoord = {x:-1, y:-1}
let endCoord = {x:-1, y:-1}
const dragBoxStyle = ref({
  left: "0px",
  top: "0px",
  width: "0px",
  height: "0px",
})

function logClickDown(event:MouseEvent) {
  mouseDown = true
  startCoord.x = event.clientX
  startCoord.y = event.clientY

  dragBoxStyle.value.width = "0px"
  dragBoxStyle.value.height = "0px"
  dragBoxStyle.value.left = startCoord.x.toString() + "px"
  dragBoxStyle.value.top = startCoord.y.toString() + "px"
}
function logClickUp(event:MouseEvent) {
  mouseDown = false
}
function mouseMoving(event:MouseEvent) {
  // 왼쪽 오른쪽 상황 바꿔서 생각할 필요 있음.
  if(mouseDown) {
    endCoord.x = event.clientX
    endCoord.y = event.clientY

    dragBoxStyle.value.width = (endCoord.x - startCoord.x).toString() + "px"
    dragBoxStyle.value.height = (endCoord.y - startCoord.y).toString() + "px"
  }
}

function startRuler() {
  if(startCoord.x>0 && endCoord.x>0) {
    // ratio = 
    router.push({ path: 'ruler', query: { ratio: length / (endCoord.x - startCoord.x) }})
  }
}
</script>


