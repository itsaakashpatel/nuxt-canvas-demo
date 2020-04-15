<template>
  <v-stage :config="configKonva"  @mousedown="handleStageMouseDown" @touchstart="handleStageMouseDown">
    <v-layer>
      <v-rect :config="configRect"/>
    </v-layer>
    <PlayGround :columns="this.columns" :bus="bus"/>
  </v-stage>
</template>

<script lang="ts">
import Vue, { PropOptions } from 'vue'
import PlayGround from '~/components/PlayGround.vue'

//INTERFACE
interface Stage {
  width : number,
  height : number
}

export default {
  components : {
    PlayGround
  },
  data() {
    return {
      configKonva: {
        width: process.client ? window.innerWidth : 700,
        height: process.client ? window.innerHeight : 700
      } as Stage,
      configRect : {
          x: 0,
          y: 0,
          width: process.client ? window.innerWidth : 700,
          height: process.client ? window.innerHeight : 700,
          fill: '#f1f1f1',
      },
      bus : new Vue()
    };
  },
  props : {
    columns : {
      type : Number
    }
  },
  methods :{

    handleStageMouseDown(e: any):void {
       (this as any).bus.$emit('handleStageMouseDown', e)
    }
  }
  
}
</script>