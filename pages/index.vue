<template>
  <v-layout
    column
    justify-center
    align-center
  >
    <div class="text-center">
         Click On Plus Icon to Add Columns
        <v-btn icon @click.prevent="dialog = true">
          <v-icon>mdi-plus</v-icon>
        </v-btn>
        <Dialog :visible="dialog"  @close="getColumns"/>
      </div>
      <Konva :columns="columns"/>
  </v-layout>
</template>

<script lang="ts">

import Vue, { PropOptions } from 'vue'

//Components
import Dialog from '~/components/Dialog.vue'
import Konva from '~/components/Konva.vue'

export default Vue.extend({
  name: 'Index',
  components : {
    Dialog,
    Konva
  },
  data () {
    return {
      dialog: false,
      columns : 1,
    }
  },

  methods : {
    getColumns(value: number) : void {
      this.dialog = false,
      this.columns = Number(value)
    }
  }
})

</script>
