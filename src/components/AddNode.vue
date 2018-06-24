<template>
  <g :transform="`translate(${x}, ${y})`" class="editor-graph-nodegroup">
    <g>
      <circle class="editor-add-node-placeholder" r="11" stroke-width="1.7"></circle>
      <g class="result-status-glyph" transform="rotate(45)">
        <polygon points="4.67 -3.73 3.73 -4.67 0 -0.94 -3.73 -4.67 -4.67 -3.73 -0.94 0 -4.67 3.73 -3.73 4.67 0 0.94 3.73 4.67 4.67 3.73 0.94 0">
        </polygon>
      </g>
    </g>
    <!--<g transform="translate(150,270)" class="editor-graph-nodegroup">-->
      <!--<g>-->
        <!--<circle class="editor-add-node-placeholder" r="11" stroke-width="1.7"></circle>-->
        <!--<g class="result-status-glyph" transform="rotate(45)">-->
          <!--<polygon points="4.67 -3.73 3.73 -4.67 0 -0.94 -3.73 -4.67 -4.67 -3.73 -0.94 0 -4.67 3.73 -3.73 4.67 0 0.94 3.73 4.67 4.67 3.73 0.94 0"></polygon>-->
        <!--</g>-->
      <!--</g>-->
      <!--<circle r="18.9" cursor="pointer" class="pipeline-node-hittarget" id="pipeline-node-hittarget-2-add" fill-opacity="0" stroke="none"></circle>-->
    <!--</g>-->
  </g>

</template>

<script>
  export default {
    name: "AddCircle",
    props: {
      x: Number,
      y: Number
    }
  }
</script>

<style scoped lang="less">
  .editor-add-node-placeholder {
    fill: #fff;
    stroke: #cbcbcb;
    cursor: pointer;
  }

  .result-status-glyph {
    stroke: none;
    fill: #fff;
  }

  .result-status-glyph polygon {
    fill: #cbcbcb;
  }


</style>
