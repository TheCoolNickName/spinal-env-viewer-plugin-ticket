<!--
  - Copyright 2019 SpinalCom - www.spinalcom.com
  -
  -  This file is part of SpinalCore.
  -
  -  Please read all of the following terms and conditions
  -  of the Free Software license Agreement ("Agreement")
  -  carefully.
  -
  -  This Agreement is a legally binding contract between
  -  the Licensee (as defined below) and SpinalCom that
  -  sets forth the terms and conditions that govern your
  -  use of the Program. By installing and/or using the
  -  Program, you agree to abide by all the terms and
  -  conditions stated or referenced herein.
  -
  -  If you do not agree to abide by these terms and
  -  conditions, do not demonstrate your acceptance and do
  -  not install or use the Program.
  -  You should have received a copy of the license along
  -  with this file. If not, see
  -  <http://resources.spinalcom.com/licenses.pdf>.
  -->

<template>
    <md-dialog :md-active="displayAddStep">
        <md-dialog-title>Create Step</md-dialog-title>
        <md-field>
            <label>Name of the step</label>
            <md-input v-model="stepName"></md-input>
        </md-field>

        <photoshop-picker v-model="stepColor"/>
        <md-dialog-actions>
            <md-button class="md-primary" v-on:click="onCancel">Close
            </md-button>
            <md-button class="md-primary" v-on:click="onConfirm">Save
            </md-button>
        </md-dialog-actions>
    </md-dialog>

</template>

<script>
  import { SpinalServiceTicket } from 'spinal-service-ticket'
  import { mapState } from 'vuex';
  import { Photoshop } from 'vue-color'

  export default {
    name: "AddStepModal",
    components: {
      'photoshop-picker': Photoshop
    },
    data: function () {
      return {
        stepName: "",
        stepColor: ""
      }
    },

    computed: mapState( ['displayAddStep', 'selectedNode'] ),

    methods: {
      onConfirm: function () {
        console.log( this.stepColor );
        const stepId =
          SpinalServiceTicket.createStep( this.stepName,
            this.stepColor.hex );
        SpinalServiceTicket.addStep( stepId, this.selectedNode.id.get() )
          .then( () => {
            this.$store.commit( 'TOGGLE_ADD_STEP' );
          } )
          .catch( ( e ) => {
            console.error( 'loris', e )
          } );
      },

      onCancel: function () {
        this.$store.commit( 'TOGGLE_ADD_STEP' )
      }
    }
  }
</script>

<style scoped>

</style>