<template>
 <div style="line-height: 0">
  <button @click="add('right')" :style="{height:node.height + 'px'}"
          class="button"
          style="vertical-align:middle">
   <span>开始 </span></button>
 </div>
</template>

<script>
  export default {
    name: "start",
    props: ['node'],
    methods: {
      add(director){
        this.$emit('task-action', 'add', this.node, director)
      }
    }
  }
</script>

<style scoped>
 .button {
  display: inline-block;
  border-radius: 4px;
  background-color: #409EFF;
  border: none;
  color: #FFFFFF;
  text-align: center;
  font-size: 14px;
  padding: 5px;
  width: 80px;
  transition: all 0.5s;
  cursor: pointer;
 }

 .button span {
  cursor: pointer;
  display: inline-block;
  position: relative;
  transition: 0.5s;
 }

 .button span:after {
  content: '»';
  position: absolute;
  opacity: 0;
  top: 0;
  right: -20px;
  transition: 0.5s;
 }


 .button:hover span {
  padding-right: 25px;
 }

 .button:hover span:after {
  opacity: 1;
  right: 0;
 }
</style>
