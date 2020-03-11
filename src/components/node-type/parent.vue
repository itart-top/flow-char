<template>
    <div style="background: silver" class="parent">
        <label> 父亲 </label>
        <slot></slot>
        <i class="right" :style="{right: '3px', top: node.height/2 -3 + 'px'}"></i>
        <i class="left" :style="{left: '3px', top: node.height/2 -3 + 'px'}"></i>
        <i class="up" :style="{top: '3px', left: node.width/2 + 'px'}"></i>
        <i class="down" :style="{bottom:'3px' , left: node.width/2 + 'px'}"></i>
    </div>
</template>

<script>
  export default {
    props: ['node'],
    name: "parent",
    methods: {
      nodeAdd (node, director) {
        const newNode = {
          id: new Date().getTime(),
          name: '开始' + '_' + this.node.subNodes.length,
          nodeType: 'sub-start',
          width: 200,
          height: 72,
          ver: 1
        }
        if (director === 'right') {
          this.node.subNodes.push(newNode)
          const nextEdges = this.node.subEdges.filter(e => e.nodeId == node.id)
          // 新节点的下一个就是原来的下一个
          nextEdges.forEach(e => {
            e.nodeId = newNode.id
          })
          this.node.subEdges.push({nodeId: node.id, nextNodeId: newNode.id})
        }
        if (director === 'left') {
          this.node.subNodes.push(newNode)
          const preEdges = this.node.subEdges.filter(e => e.nextNodeId == node.id)
          if (preEdges.length !== 0) {
            // 新节点的下一个就是原来的下一个
            preEdges.forEach(e => {
              e.nextNodeId = newNode.id
            })
          }
          this.node.subEdges.push({nodeId: newNode.id, nextNodeId: node.id})
        }
        if (director === 'up') {
          this.node.subNodes.unshift(newNode)
          const preEdges = this.node.subEdges.filter(e => e.nextNodeId == node.id)
          const nextEdges = this.node.subEdges.filter(e => e.nodeId == node.id)
          preEdges.forEach(pe => {
            this.node.subEdges.push({nodeId: pe.nodeId, nextNodeId: newNode.id})
          })
          nextEdges.forEach(ne => {
            this.node.subEdges.push({nodeId: newNode.id, nextNodeId: ne.nextNodeId})
          })
        }
        if (director === 'down') {
          this.node.subNodes.push(newNode)
          const preEdges = this.node.subEdges.filter(e => e.nextNodeId == node.id)
          const nextEdges = this.node.subEdges.filter(e => e.nodeId == node.id)
          nextEdges.forEach(ne => {
            this.node.subEdges.push({nodeId: newNode.id, nextNodeId: ne.nextNodeId})
          })
          preEdges.forEach(pe => {
            this.node.subEdges.push({nodeId: pe.nodeId, nextNodeId: newNode.id})
          })
        }
        this.node.ver = this.node.ver + 1
      }
    }
  }
</script>

<style scoped>
    .parent:hover i {
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
