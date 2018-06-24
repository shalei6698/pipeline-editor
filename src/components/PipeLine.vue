<template>
  <svg :width="width" :height="height">
    <template v-for="(stage, idx) in data.stages">
      <path class="pipeline-connector placeholder" :stroke-width="lineWidth"
            :key="'l1' + idx"
            :d="`M ${offset.x + lineLength * idx} ${offset.y}
                 l 60 0 ${c2} l 0 ${46 + 70*(stage.parallel ? stage.parallel.length-1 : 0)} ${c4} l 36 0`"
            fill="none">
      </path>
      <!--left-->
      <path v-if="stage.parallel" v-for="(parallel, pidx) in stage.parallel.slice(1)"
            :key="'l2' + idx + ' ' + pidx"
            class="pipeline-connector" :stroke-width="lineWidth"
            :d="`M ${offset.x + lineLength * idx} ${offset.y}
                 l 60 0 ${c2} l 0 ${46 + 70*pidx} ${c4} l 36 0`"
            fill="none">
      </path>
      <!--right-->
      <path v-if="stage.parallel && idx < data.stages.length - 1"
            v-for="(parallel, pidx) in stage.parallel.slice(1)"
            :key="'l3' + idx + ' ' + pidx"
            class="pipeline-connector" :stroke-width="lineWidth"
            :d="`M ${offset.x + lineLength * (idx + 1)} ${offset.y + 70 * (pidx + 1)}
                 l 36 0 ${c3} l 0 -${46 + 70*pidx} ${c1} l 60 0`"
            fill="none">
      </path>
      <line class="pipeline-connector" :stroke-width="lineWidth" :key="'l4' + idx"
          :x1="offset.x + idx * lineLength" :y1="offset.y"
          :x2="offset.x + (idx + 1) * lineLength" :y2="offset.y">
      </line>

    </template>

    <line class="pipeline-connector placeholder" stroke-width="3.2"
      :x1="offset.x + data.stages.length * lineLength" :y1="offset.y"
      :x2="offset.x + (data.stages.length + 1) * lineLength" :y2="offset.y"></line>

    <template v-for="(stage, idx) in data.stages">
      <!--nodes-->
      <StepNode v-for="(parallel, pidx) in stage.parallel" :key="idx + ' ' + pidx"
                :x="offset.x + lineLength * (idx + 1)"
                :y="offset.y + 70 * pidx">
      </StepNode>

      <AddNode :x="offset.x + lineLength*(idx + 1)"
               :y="offset.y + 70*(stage.parallel ? stage.parallel.length : 0)"
               :key="`add` + idx">
      </AddNode>
    </template>

    <g transform="translate(30,60)" class="editor-graph-nodegroup">
      <circle r="7.5" class="start-node" stroke="none"></circle>
      <circle r="18.9" cursor="pointer" class="pipeline-node-hittarget" id="pipeline-node-hittarget-1-start" fill-opacity="0" stroke="none"></circle>
    </g>
    <AddNode :x="offset.x + lineLength*(data.stages.length + 1)"
             :y="offset.y">
    </AddNode>

  </svg>
</template>

<script>
import AddNode from './AddNode'
import StepNode from './StepNode'

export default {
  name: 'PipeLine',
  components: { AddNode, StepNode },
  props: {
    value: Object
  },
  data () {
    return {
      lineLength: 120,
      lineWidth: 3.2,
      offset: {
        x: 30,
        y: 60
      },
      c1: 'c 0 -12 12 -12 12 -12',
      c2: 'c 12 0 12 12 12 12',
      c3: 'c 12 0 12 -12 12 -12',
      c4: 'c 0 12 12 12 12 12',

      data: {
        type: 'any',
        stages: [
          {
            "name": "a",
            "parallel": [
              {
                "name": "a",
                "branches": [
                  {
                    "name": "default",
                    "steps": [
                      {"name":"sh","arguments":[{"key":"script","value":{"isLiteral":true,"value":"ls "}}]}
                    ]
                  }
                ]
              },
              {
                "name": "b",
                "branches": [
                  {"name": "default", "steps": [{"name":"echo","arguments":[{"key":"message","value":{"isLiteral":true,"value":"xxx"}}]}]}
                ]
              }
            ]
          },
          {"name":"e","parallel":[{"name":"e","branches":[{"name":"default","steps":[{"name":"sh","arguments":[{"key":"script","value":{"isLiteral":true,"value":"ls"}}]}]}]},{"name":"error","branches":[{"name":"default","steps":[{"name":"echo","arguments":[{"key":"message","value":{"isLiteral":true,"value":"aaa"}}]}]}]}]}]
      }
    }
  },
  computed: {
    width () {
      return 500
    },
    height () {
      return 500
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less" scoped>
  @line-color: #949393;
  @placeholder-color: #eae9e9;

  .pipeline-connector {
    stroke: @line-color;
    &.placeholder {
      stroke: @placeholder-color;
    }
  }

  .start-node {
    fill: #cdcdcd;
  }

</style>
