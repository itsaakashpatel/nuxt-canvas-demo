<template>
  <v-row justify="center">
    <v-dialog v-model="show" persistent max-width="290">
      <v-card>
        <v-card-title class="headline">Hello</v-card-title>
        <v-card-text>
           <v-text-field
            label="Add columns"
            placeholder="Add no. of columns in a row"
            outlined
            type="number"
            min="1"
            v-model="columns"
          ></v-text-field>
        </v-card-text>
        <v-card-actions>
          <v-btn color="green darken-1" text @click="closeDialog()">Done</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
</template>

<script lang="ts">
  import Vue, { PropOptions } from 'vue'
  export default Vue.extend({
    name: "Dialog",
    data () {
      return {
        columns : 1
      }
    },

    props : {
      visible: {
        type : Boolean
      } as PropOptions<Boolean>
    },

    computed : {
      show : {
        get() : Boolean {
          return this.visible
        }
        
      }
    },
    methods : {
      closeDialog() : void {
        this.$emit("close", this.columns)
        this.columns = 1
      }
    }
  })
</script>>
