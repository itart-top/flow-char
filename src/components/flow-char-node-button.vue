<template>
    <span class="node-button">
        <i class="right" :style="{right: '3px', top: node.height/2 -3 + 'px'}"></i>
        <i class="left" :style="{left: '3px', top: node.height/2 -3 + 'px'}"></i>
        <i class="up" :style="{top: '3px', left: node.width/2 + 'px'}"></i>
        <i class="down" :style="{bottom:'3px' , left: node.width/2 + 'px'}"></i>
    </span>
</template>

<script>
  export default {
    name: 'flow-char-node-button',
    props: ['node']
}
</script>

<style scoped>
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
