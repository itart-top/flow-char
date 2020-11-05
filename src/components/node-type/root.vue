<template>
 <div v-if="node.subNodes.length === 0">
  <a href="javascript:void(0)" class="root-add" @click="onAdd"> + </a>
 </div>
 <div v-else>
  <slot></slot>
 </div>
</template>

<script>
  export default {
    name: "root",
    props: ['node'],
    methods: {
      onAdd() {
        this.$emit('task-action', 'add', this.node, 'sub')
      }
    }
  }
</script>

<style scoped>
 .root-add {
  text-decoration: none;
  font-size: xx-large;
 }
</style>
