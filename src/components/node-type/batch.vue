<template>
    <div style="border: 1px solid #ebeef5; background-color: #fff;color: #303133;box-shadow: 0 2px 12px 0 rgba(0,0,0,.1); transition: .3s;" class="batch">
        <div class ="range-name">
            {{node.name}}
            <slot></slot>
        </div>
    </div>
</template>
<script>
  export default {
    name: "batch",
    props: ['node'],
  }
</script>
<style scoped>
    .range-name{
        position: absolute;
        top: -10px;
        left: 10px;
        font-size: 12px;
        padding: 2px;
        border-radius: 3px;
        color: white;
        background-color: lightseagreen;
    }
</style>
