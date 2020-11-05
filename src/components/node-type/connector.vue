<template>
    <div>
        <div class ="connector">
            <a href="javascript:void(0)" @click="add"> + </a>
        </div>
    </div>
</template>
<script>
  export default {
    name: "Connector",
    props: ['node'],
    methods: {
      add(director) {
        this.$emit('task-action', 'add', this.node, director)
      }
    }
  }
</script>
<style scoped>
    .connector{
        border-radius: 10px;
        text-align: center;
        line-height: 20px;
        background-color: #63c0a1;
    }
    .connector a {
        text-decoration: none;
        color: #fff;
    }
</style>
