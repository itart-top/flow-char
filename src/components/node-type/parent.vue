<template>
    <div class="sub-start" :class="{ 'selected': selectedId === node.id }"
         :style="{ lineHeight: this.node.height + 'px' }">
        <div v-if="node.subNodes.length !== 0 "
            style="line-height: 15px"
            class="range-name">
            <a href="javascript:void(0)" @click="edit">{{node.name}}</a>
        </div>
        <slot></slot>
        <a v-if="node.subNodes.length === 0 "
           href="javascript:void(0)"
           @click="add('sub')">
            +
        </a>
        <a class = "del-btn" href="javascript:void(0)" @click="del">X</a>
        <a :style="{ lineHeight: this.node.height + 'px' }"
           class="arrow left" href="javascript:void(0)" @click="add('left')">
            +
        </a>
        <a :style="{ lineHeight: this.node.height + 'px' }"
           class="arrow right" href="javascript:void(0)" @click="add('right')">
            +
        </a>
        <a :style="{ lineHeight: '15px' }"
           class="arrow top" href="javascript:void(0)" @click="add('top')">
            +
        </a>
        <a :style="{ lineHeight: '15px' }"
           class="arrow bottom" href="javascript:void(0)" @click="add('bottom')">
            +
        </a>
    </div>
</template>

<script>
  export default {
    props: ['node', 'selectedId'],
    name: "Node",
    methods: {
      add (director) {
        this.$emit('task-action', 'add', this.node, director)
      },
      del () {
        this.$emit('task-action', 'del', this.node)
      },
      edit () {
        this.$emit('task-action', 'edit', this.node)
      }
    }
  }
</script>

<style scoped>
    .range-name{
        position: absolute;
        top: 0;
        left: 0;
        font-size: 12px;
        background-color: lightseagreen;
    }
    .range-name > a{
        color: white;
        padding: 2px;
        font-weight: bold;
        display: block;
    }
    .sub-start{
        transition: .3s;
        text-align: center;
        box-shadow: 2px 2px 19px #aaa;
        -o-box-shadow: 2px 2px 19px #aaa;
        -webkit-box-shadow: 2px 2px 19px #aaa;
        -moz-box-shadow: 2px 2px 19px #aaa;
        border-radius: .1em;
    }
    .sub-start.selected{
        background-color: #EBEEF5;
    }
    .sub-start.selected > .range-name{
        background-color: #67C23A;
    }
    .del-btn{
        position: absolute;
        right: 10px;
        top: 5px;
        line-height: 20px;
        transition: 0.3s;
        opacity: 0;
        color: darkgray;
    }
    .del-btn:hover {
        color: brown;
    }

    .sub-start:hover > .arrow, .sub-start:hover > .del-btn {
        opacity: 1;
        z-index: 2;
    }

    .arrow {
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

    .arrow.right {
        top: 0;
        bottom: 0;
        right: -15px;
    }
    .arrow.left {
        top: 0;
        bottom: 0;
        left: -15px;
    }
    .arrow.top{
        width: 100%;
        height: 15px;
        line-height: 15px;
        left: 0;
        text-align: center;
        right: 0;
        top: -15px;
    }
    .arrow.bottom{
        width: 100%;
        height: 15px;
        line-height: 15px;
        left: 0;
        text-align: center;
        right: 0;
        bottom: -15px;
    }
</style>
