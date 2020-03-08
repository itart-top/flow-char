<template>
  <div>
      <flow-char-node id ="flow_char_root"
        :node ="node" :style="{ width: '100%', height: node.height + 'px', position: 'relative'}">
      </flow-char-node>
  </div>
</template>

<script>
import FlowCharNode from './flow-char-node'
import dagre from 'dagre'
import { jsPlumb } from 'jsplumb'
export default {
  name: 'flow-char',
  components: { FlowCharNode },
  props: ['node'],
  data() {
    return {
      jp: undefined
    }
  },
  created() {
    this.layout(this.node)
  },
  methods: {
    empty2array(arr) {
      return arr || []
    },
    layout(node) {
      if (!node.subNodes || node.subNodes.length === 0) {
        return
      }
      const g = new dagre.graphlib.Graph({ directed: true })
      g.setGraph({
        rankdir: 'LR', nodesep: 16, ranksep: 30, rangker: 'network-simplex',
        marginx: 20, marginy: 20
      })
      g.setDefaultEdgeLabel(function () {
        return {}
      })
      this.empty2array(node.subNodes).forEach(n => {
        this.layout(n)
        g.setNode(n.id, {
          lable: n.name,
          width: n.width,
          height: n.height,
          data: n
        })
      })
      this.empty2array(node.subEdges).forEach(e => {
        g.setEdge(e.nodeId, e.nextNodeId, e)
      })
      dagre.layout(g)
      g.nodes().forEach(n => {
        const node = g.node(n)
        const data = node.data
        Object.assign(data, {width: node.width, height: node.height, x: node.x, y: node.y})
      })
      g.edges()
      node.height = g.graph().height
      node.width = g.graph().width
    },
    nearly(v1, v2) {
      const diff = v1 - v2
      return diff < 1 && diff > -1
    },
    anchor(node, x, y) {
      if (this.nearly(node.y - 0.5 * node.height, y)) {
        return 'TopCenter'
      }
      if (this.nearly(node.y + 0.5 * node.height, y)) {
        return 'BottomCenter'
      }
      if (this.nearly(node.x + 0.5 * node.width, x)) {
        return 'Right'
      }
      if (this.nearly(node.x - 0.5 * node.width, x)) {
        return 'Left'
      }
      console.log(node, x, y)
      return 'Right'
    },
    connect(node) {
      this.empty2array(node.subEdges).forEach(e => {
        const _anchor = []
        let _node = node.subNodes.find(n => n.id === e.nodeId)
        _anchor.push(this.anchor(_node, e.points[0].x, e.points[0].y))
        _node = node.subNodes.find(n => n.id === e.nextNodeId)
        const endPoint = e.points[e.points.length - 1]
        _anchor.push(this.anchor(_node, endPoint.x, endPoint.y))
        this.jp.connect({
          source: 'node_' + e.nodeId,
          target: 'node_' + e.nextNodeId,
          anchor: _anchor,
          overlays: [['Arrow', { width: 9, length: 9, location: 1 }], ['Label', { label: ''}]]
        })
      })
      this.empty2array(node.subNodes).forEach(n => {
        this.connect(n)
      })
    }
  },
  mounted() {
    jsPlumb.ready(() => {
      this.jp = jsPlumb.getInstance({
        DragOptions: { cursor: 'pointer', zIndex: 2000 },
        EndpointStyle: { radius: 0.11 }, // 端点样式
        PaintStyle: { strokeWidth: 2, stroke: '#ffa500' },
        EndpointHoverStyle: { fill: '#ffa500' },
        Connector: ['Flowchart', { stub: 10 }],
        Container: 'flow_char_root'
      })
      setTimeout(() => {
        this.connect(this.node)
      }, 1000)
    })
  }
}
</script>

<style scoped>

</style>
