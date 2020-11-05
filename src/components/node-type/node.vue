<template>
    <div class="sub-start" :class="{ 'selected': selectedId === node.id }"
    :style="{ lineHeight: this.node.height + 'px' }">
        <a class="inner" href="javascript:void(0)" @click="edit">{{ node.name }}</a>
        <a class = "del-btn inner" href="javascript:void(0)" :style="{ lineHeight: this.node.height + 'px' }"
         @click="del">X</a>
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
    .sub-start{
        transition: .3s;
        text-align: center;
        box-shadow: 2px 2px 19px #aaa;
        background-color: #909399;
        -o-box-shadow: 2px 2px 19px #aaa;
        -webkit-box-shadow: 2px 2px 19px #aaa;
        -moz-box-shadow: 2px 2px 19px #aaa;
        border-radius: .1em;
    }
    .sub-start:hover{
        background-color: #303133;
    }
    .sub-start.selected{
        background-color: #67C23A;
    }
    .sub-start.selected > .inner{
       color: #fff;
    }
    .sub-start a {
        text-decoration: none;
        font-size: 12px;
        color: #fff;
    }
    .sub-start > .del-btn{
        opacity: 0;
        z-index: 1;
        transition: 0.5s;
        position: absolute;
        right: 10px;
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
