<template>
    <div>
        <div style="font-size: 14px">
            布局：
            <select v-model="settings.ranker">
                <option value="network-simplex">
                    network-simplex
                </option>
                <option value="tight-tree">
                    tight-tree
                </option>
                <option value="longest-path">
                    longest-path
                </option>
            </select>
            连接样式
            <select v-model="settings.connector[0]">
                <option value="Bezier">贝塞尔曲线</option>
                <option value="Flowchart">
                    90度流程线
                </option>
                <option value="StateMachine">
                    状态机
                </option>
                <option value="Straight">
                    直线
                </option>
            </select>
            stub:
            <input type="text" v-model="settings.connector[1].stub"/>
        </div>
        <div id="app" style="overflow: auto; width: 100%">
            <flow-char :node="task"
                       :settings="settings"
                        :key="task.ver"
                       :selected-id="editNode? editNode.id : undefined"
                       @task-action="onAction"/>
        </div>
        <div class="el-divider el-divider--horizontal"> </div>
        <div v-if="editNode" class="form">
            <div class="form-item">
                <label> 名称</label>
                <input type="text" v-model="editNode.name">
            </div>
            <div class="form-item">
                <label>类型</label>
                <select v-model="editNode.nodeType">
                    <option value="node">子任务</option>
                    <option value="parent">父任务</option>
                </select>
            </div>
        </div>
    </div>

</template>

<script>
  import FlowChar from './components/flow-char'

  export default {
    name: 'App',
    components: {
      FlowChar
    },
    data () {
      return {
        editNode: undefined,
        settings: {
          ranker: 'network-simplex',
          connector: ['Flowchart', { stub: 10 }]
        },
        task: {
          id: 'char_root',
          name: 'root',
          nodeType: 'root',
          ver: 1,
          subNodes: [],
          subEdges: []
        }
      }
    },
    watch: {
      settings: {
        handler() {
          this.refresh()
        },
        immediate: true,
        deep: true
      }
    },
    methods: {
      onAction(evt, node, direction){
        if (evt === 'add'){
          this.editNode = this.onAddTask(node, direction)
        } else if (evt === 'del'){
          if (this.edgeMode && this.edgeMode.id === node.id){
            this.editNode = undefined
          }
          this.delTask(node)
        } else if (evt === 'edit'){
          this.editNode = node
        }
        this.refresh()
      },
      delTask(node){
        const nextEgs = node.parent.subEdges.filter(e => e.nodeId === node.id)
        const preEgs = node.parent.subEdges.filter(e => e.nextNodeId === node.id)
        let connectorNodes = []
        preEgs.forEach(preEg => {
          const nn = node.parent.subNodes.filter(n => n.id === preEg.nodeId && n.nodeType === 'connector')
          connectorNodes.push(...nn)
        })
        nextEgs.forEach(nextEg => {
          const nn = node.parent.subNodes.filter(n => n.id === nextEg.nextNodeId && n.nodeType === 'connector')
          connectorNodes.push(...nn)
        })
        if (nextEgs.length > 1 && preEgs.length > 1) {
          node.nodeType = 'connector'
          node.shap = 'circle'
          node.width = 20
          node.height = 20
        } else {
          node.parent.subNodes = node.parent.subNodes.filter(sub => sub.id !== node.id)
          node.parent.subEdges = node.parent.subEdges.filter(e => e.nodeId !== node.id && e.nextNodeId !== node.id)
          if (nextEgs.length === 1) {
            preEgs.forEach(preEg => {
              preEg.nextNodeId = nextEgs[0].nextNodeId
              if (!this.isConnected(node.parent.subEdges, preEg.nodeId, preEg.nextNodeId)) {
                node.parent.subEdges.push(preEg)
              }
            })
          } else if (preEgs.length === 1) {
            nextEgs.forEach(nextEg => {
              nextEg.nodeId = preEgs[0].nodeId
              if (!this.isConnected(node.parent.subEdges, nextEg.nodeId, nextEg.nextNodeId)) {
                node.parent.subEdges.push(nextEg)
              }
            })
          }
          connectorNodes.forEach(cn => {
            this.delTask(cn)
          })
        }
      },
      isConnected(edges, srcId, targetId) {
        const matchEdges = edges.filter(e => e.nodeId === srcId)
        for (let i = 0, size = matchEdges.length; i< size; i++){
          const me = matchEdges[i]
          if (me.nextNodeId === targetId){
            return true
          }
          const ok = this.isConnected(edges, me.nextNodeId, targetId)
          if (ok){
            return true
          }
        }
        return false
      },
      refresh() {
        this.task.ver = this.task.ver + 1
      },
      onAddTask(node, direction){
        const newNode = {
          id: new Date().getTime(), name: '为命名',
          subNodes: [],
          subEdges: [],
          parent: node.parent,
          nodeType: 'node',
          width: 100,
          height: 30,
          ver: 1
        }
        if (node.nodeType === 'connector'){
          node.nodeType = 'node'
          node.shape = undefined
          node.width = 100
          node.height = 30
        } else if (direction === 'sub'){
          newNode.parent = node
          newNode.nodeType = 'start'
          newNode.width = 80
          node.subNodes.push(newNode)
          const endNode = {
            id: 'end-' + new Date().getTime(),
            name: 'end',
            subNodes: [],
            subEdges: [],
            parent: node,
            shape: 'circle',
            nodeType: 'end',
            width: 34,
            height: 34,
            ver: 1
          }
          node.subNodes.push(endNode)
          node.subEdges.push({ nodeId: newNode.id, nextNodeId: endNode.id })
        } else if (direction === 'right'){
          this.addRight(node, newNode)
        } else if(direction === 'left'){
          this.addLeft(node, newNode)
        } else {
          this.addUpDown(node, newNode)
        }
        return newNode
      },
      addRight(node, newNode){
        node.parent.subNodes.push(newNode)
        const nextEdges = node.parent.subEdges.filter(e => e.nodeId === node.id)
        nextEdges.forEach(e => {
          e.nodeId = newNode.id
        })
        node.parent.subEdges.push({ nodeId: node.id, nextNodeId: newNode.id })
      },
      addLeft(node, newNode) {
        node.parent.subNodes.push(newNode)
        const preEdges = node.parent.subEdges.filter(e => e.nextNodeId === node.id)
        preEdges.forEach(e => {
          e.nextNodeId = newNode.id
        })
        node.parent.subEdges.push({ nodeId: newNode.id, nextNodeId: node.id })
      },
      addUpDown(node, newNode){
        node.parent.subNodes.unshift(newNode)
        const preEdges = node.parent.subEdges.filter(e => e.nextNodeId === node.id)
        const nextEdges = node.parent.subEdges.filter(e => e.nodeId === node.id)
        if (preEdges.length === 1){
          node.parent.subEdges.push({ nodeId: preEdges[0].nodeId, nextNodeId: newNode.id })
        } else if(preEdges.length > 1){
          const connectorNode = {
            id: 'conn_pre_' + new Date().getTime(),
            name: '连接点' + '_' + node.parent.subNodes.length,
            nodeType: 'connector',
            subNodes: [],
            subEdges: [],
            parent: node.parent,
            width: 20,
            height: 20,
            shape: 'circle',
            ver: 1
          }
          preEdges.forEach(pe => {
            pe.nextNodeId = connectorNode.id
          })
          node.parent.subNodes.unshift(connectorNode)
          node.parent.subEdges.push({ nodeId: connectorNode.id, nextNodeId: newNode.id })
          node.parent.subEdges.push({ nodeId: connectorNode.id, nextNodeId: node.id })
        }
        if (nextEdges.length === 1) {
          node.parent.subEdges.push({ nodeId: newNode.id, nextNodeId: nextEdges[0].nextNodeId })
        } else if (nextEdges.length > 1){
          const connectorNode = {
            id: 'conn_next_' + new Date().getTime(),
            name: '连接点' + '_' + node.parent.subNodes.length,
            nodeType: 'connector',
            subNodes: [],
            subEdges: [],
            parent: node.parent,
            width: 20,
            height: 20,
            shape: 'circle',
            ver: 1
          }
          node.parent.subNodes.unshift(connectorNode)
          nextEdges.forEach(ne => {
            ne.nodeId = connectorNode.id
          })
          node.parent.subEdges.push({ nodeId: newNode.id, nextNodeId: connectorNode.id })
          node.parent.subEdges.push({ nodeId: node.id, nextNodeId: connectorNode.id })
        }
      }
    }
  }
</script>

<style>
    .form{
        font-size: 14px;
    }
    .form .form-item{
        margin-bottom: 10px;
    }
    .form .form-item label{
        width: 200px;
    }
    a {
        text-decoration: none;
    }
    .el-divider--horizontal{
        display: block;
        height: 1px;
        width: 100%;
        margin: 24px 0;
    }
    .el-divider{
        background-color: #dcdfe6;
        position: relative;
    }
</style>
