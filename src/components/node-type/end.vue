<template>
 <div class="sub-start">
  <div class="start_btn"></div>
  <a :style="{ lineHeight: node.height + 'px'}"
     class="arrow left" href="javascript:void(0)" @click="add('left')">
  +
  </a>
 </div>
</template>

<script>
  export default {
   props: ['node'],
    name: "end",
    methods: {
     add(director){
       this.$emit('task-action', 'add', this.node, director)
     }
    }
  }
</script>

<style scoped>
 .start_btn{
  width: 34px;
  height: 34px;
  background-color: #63c0a1;
  border-radius: 50%;
 }
 .sub-start > a {
  text-decoration: none;
 }
 .start_btn:after{
  content: "";
  display: block;
  width: 16px;
  height: 16px;
  border-radius: 50%;
  background-color: white;
  position: relative;
  top: 9px;
  left: 9px;
 }
 .sub-start:hover > .arrow, .sub-start:hover{
  opacity: 1;
 }
 .arrow{
  position: absolute;
  opacity: 0;
  z-index: 1;
  cursor: pointer;
  background: gainsboro;
  transition: 0.5s;
  width: 15px;
  color: darkgray;
 }
 .arrow:hover{
  background: burlywood;
  color: brown;
 }
 .arrow.left {
  top:0;
  bottom: 0;
  left: -15px;
 }
</style>
