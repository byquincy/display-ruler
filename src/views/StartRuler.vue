<template>
    <div
    @mousedown="logClickDown"
    @mousemove="mouseMoving"
    @mouseup="logClickUp"
    id="app-container">
        <span class="now-length" :style="[nowLengthStyle.x, nowLengthCommon]">{{ nowLength.y }}</span>
        <span class="now-length" :style="[nowLengthStyle.y, nowLengthCommon]">{{ nowLength.x }}</span>

        <div id="app-drag-box" :style="dragBoxStyle"></div>
    </div>
  </template>
  
<script setup lang="ts">
import { ref } from 'vue';
import { useRoute } from 'vue-router'

const route = useRoute()
const ratio = parseFloat(route.query.ratio?.toString() ?? "0")  
const nowLength = ref({ x:0, y:0 })
const nowLengthStyle = ref({ 
    x: { left: "0px", top: "0px", width:"auto"},
    y: { left: "0px", top: "0px", width:"auto"},
})
const nowLengthCommon = ref({ display:"none" })

let mouseDown = false
let startCoord = {x:-1, y:-1}
let endCoord = {x:-1, y:-1}
const square = {x:-1, y:-1}
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

    nowLengthCommon.value.display = "none"
}
function logClickUp(event:MouseEvent) {
    mouseDown = false
}
function mouseMoving(event:MouseEvent) {
    // 왼쪽 오른쪽 상황 바꿔서 생각할 필요 있음.
    if(mouseDown) {
        nowLengthCommon.value.display = "block"

        endCoord.x = event.clientX
        endCoord.y = event.clientY

        square.x = endCoord.x - startCoord.x
        square.y = endCoord.y - startCoord.y
        nowLength.value.x = Math.round(square.x * ratio)
        nowLength.value.y = Math.round(square.y * ratio)

        dragBoxStyle.value.width = square.x.toString() + "px"
        dragBoxStyle.value.height = square.y.toString() + "px"

        nowLengthStyle.value.x.left = startCoord.x.toString() + "px"
        nowLengthStyle.value.x.width = square.x.toString() + "px"
        nowLengthStyle.value.x.top = endCoord.y.toString() + "px"

        nowLengthStyle.value.y.left = endCoord.x.toString() + "px"
        nowLengthStyle.value.y.top = "calc(" + endCoord.y.toString() + "px - 1rem)"
    }
}
</script>


