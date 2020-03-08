<template>
 <div style="background: cornflowerblue" class="middle">
  <label> 结束 </label>
  <i class="right" :style="{right: 0, top: node.height/2 + 'px'}"></i>
  <i class="left" :style="{left: 0, top: node.height/2 + 'px'}"></i>
  <i class="up" :style="{top: 0, left: node.width/2 + 'px'}"></i>
  <i class="down" :style="{bottom: 0, left: node.width/2 + 'px'}"></i>
 </div>
</template>

<script>
  export default {
   props: ['node'],
    name: "end"
  }
</script>

<style scoped>
 .middle:hover i {
  opacity: 1
 }
 i {
  border: solid black;
  border-width: 0 3px 3px 0;
  display: inline-block;
  padding: 3px;
  opacity: 0;
  cursor: pointer;
  position: absolute;
  transition: 0.5s;
 }

 .right {
  transform: rotate(-45deg);
  -webkit-transform: rotate(-45deg);
 }

 .left {
  transform: rotate(135deg);
  -webkit-transform: rotate(135deg);
 }

 .up {
  transform: rotate(-135deg);
  -webkit-transform: rotate(-135deg);
 }

 .down {
  transform: rotate(45deg);
  -webkit-transform: rotate(45deg);
 }
</style>
