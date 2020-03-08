<template>
    <div>
    <button v-on:click="refresh">刷新</button>
    <div id="app" style="overflow: auto; width: 100%; height: 500px">
        <flow-char :node="task" :key="task.ver"/>
    </div>
    </div>
</template>

<script>
  import FlowChar from './components/flow-char.vue'

  export default {
    name: 'App',
    data () {
      return {
        task: {
          id: 1,
          name: 'root',
          nodeType: 'root',
          ver: 1,
          subNodes: [{id: 2, name: '开始', nodeType: 'start', width: 200, height: 72, ver: 1}, {
            id: 3,
            name: '结束',
            nodeType: 'end',
            ver: 1,
            width: 120,
            height: 80
          }, {
            id: 4,
            name: '父亲',
            nodeType: 'parent',
            ver: 1,
            subNodes: [{id: 5, name: '开始11', nodeType: 'sub-start', width: 200, height: 72, ver: 1},
              {id: 7, name: '开始', nodeType: 'middle', width: 200, height: 72, ver: 1},
              { id: 6, name: '结束', nodeType: 'end', ver: 1, width: 120, height: 80}],
            subEdges: [{nodeId: 5, nextNodeId: 7}, {nodeId: 7, nextNodeId: 6}]
          }],
          subEdges:
            [{nodeId: 2, nextNodeId: 3}, {nodeId: 3, nextNodeId: 4}]
        }
      }
    },
    components: {
      FlowChar
    },
    methods: {
      refresh() {
        this.task.ver = this.task.ver + 1
      }
    }
  }
</script>

<style>

</style>
