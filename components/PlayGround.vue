<template>
    <v-layer ref="layer">
      <v-rect  v-for="item in gridItems" :key="item.id" :config="item" @transformend="handleTransformEnd"/>
      <v-transformer ref="transformer" />
    </v-layer>
</template>

<script lang="ts">
import Vue, { PropOptions } from 'vue'
import Konva from 'konva';

interface Config {
  id: string,
  x: number,
  y: number,
  width: number,
  height: number,
  fill: string,
  rotation : number,
  scaleX : number,
  scaleY : number,
  name : string,
  draggable : boolean
}


export default Vue.extend({
  name: 'Grid',
  data() {
    return {
      gridItems : new Array<Config>(),
      selectedShapeName: new String('')
    }
  },
  props : {
    columns: {
        type : Number
      } as PropOptions<Number>,
    bus: {}
  },
  methods :{
    addColumns():void {
      let columnWidth= Math.ceil((process.client ? window.innerWidth : 700) / +this.columns);
      //ADD COLUMNS TO LAYER
      this.gridItems.splice(0, this.gridItems.length)
      for (let index = 0; index < this.columns; index++) {
          let obj = {
              id: `rect-${index}`,
              x: index * 100,
              y: index * 100 / 2,
              width: columnWidth,
              height: columnWidth / 2,
              fill: '#cdcdcd',
              rotation : 0,
              scaleX : 1,
              scaleY : 1,
              name :  `rect-${index}`,
              draggable: true
          } as Config
          this.gridItems.push(obj)
      }
    },
    handleTransformEnd(e: any):void {
      let rect: Config = this.gridItems.find(r => r.name === this.selectedShapeName)!;
      // UPDATE THE OBJECT
      rect.x = e.target.x();
      rect.y = e.target.y();
      rect.rotation = e.target.rotation();
      rect.scaleX = e.target.scaleX();
      rect.scaleY = e.target.scaleY();

      // CHANGE FILL TO RANDOM COLOR
      rect.fill = Konva.Util.getRandomColor();
    },
    handleStageMouseDown(e: any):void {
       // clicked on layer - clear selection
      if (e.target === e.target.getStage()) {
        this.selectedShapeName = '';
        this.updateTransformer();
        return;
      }

      // clicked on transformer - do nothing
      const clickedOnTransformer =
        e.target.getParent().className === 'Transformer';
      if (clickedOnTransformer) {
        return;
      }

      // find clicked rect by its name
      const name = e.target.name();
      const rect = this.gridItems.find(r => r.name === name);
      if (rect) {
        this.selectedShapeName = name;
      } else {
        this.selectedShapeName = '';
      }
      this.updateTransformer();
    },
    updateTransformer():void {
      // here we need to manually attach or detach Transformer node
      const transformerNode = (this as any).$refs.transformer.getNode();
      const stage = transformerNode.getStage();
      const { selectedShapeName } = this;

      const selectedNode = stage.findOne('.' + selectedShapeName);
      // do nothing if selected node is already attached
      if (selectedNode === transformerNode.node()) {
        return;
      }

      if (selectedNode) {
        // attach to another node
        transformerNode.attachTo(selectedNode);
      } else {
        // remove transformer
        transformerNode.detach();
      }
      transformerNode.getLayer().batchDraw();
    }
  },
  watch : {
    columns : function() : void {
      //ADD COLUMNS TO LAYER on Change
      this.addColumns()
    }
  },
  mounted() {
    (this as any).bus.$on('handleStageMouseDown', this.handleStageMouseDown)
  }

})
</script>