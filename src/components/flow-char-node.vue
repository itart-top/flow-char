<template>
    <component :is="comp" :node="node" :style="nodeStyle" :id="'node_' + node.id"
               :class="['node-window', node.nodeType.replace(':',' ')]">
        <slot>
            <flow-char-node v-for="n in node.subNodes"
                            :node="n"
                            :key="n.id+ '_' + n.ver">
            </flow-char-node>
        </slot>
    </component>
</template>

<script>
  export default {
    name: 'flow-char-node',
    props: ['node'],
    computed: {
      nodeStyle () {
        return {
          left: (this.node.x - this.node.width / 2) + 'px',
          top: (this.node.y - this.node.height / 2) + 'px',
          height: this.node.height + 'px',
          width: this.node.width + 'px',
          position: 'absolute'
        }
      },
      comp () {
        return () => import(`./node-type/${this.node.nodeType.replace(':', '/')}.vue`)
      }
    }
}
</script>
