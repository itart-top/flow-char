<template>
    <div style="background: cornflowerblue" class="sub-start">
        <label> {{node.name}} </label>
        <i class="right" :style="{right: 0, top: node.height/2 + 'px'}" v-on:click="add('right')"></i>
        <i class="left" :style="{left: 0, top: node.height/2 + 'px'}" v-on:click="add('left')"></i>
        <i class="up" :style="{top: 0, left: node.width/2 + 'px'}" v-on:click="add('up')"></i>
        <i class="down" :style="{bottom: 0, left: node.width/2 + 'px'}" v-on:click="add('down')"></i>
    </div>
</template>

<script>
  export default {
    props: ['node'],
    name: "sub-start",
    methods: {
      add (director) {
        this.$parent.$parent.nodeAdd(this.node, director)
      }
    }
  }
</script>

<style scoped>
    .sub-start:hover i {
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
