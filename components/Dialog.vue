<template>
  <v-dialog
      v-model="getDialog.isShow"
      width="500"
      persistent
    >
      <v-card>
        <v-card-title class="headline grey lighten-2">
          {{getDialog.title}}
        </v-card-title>

        <v-card-text v-html="getDialog.message"></v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="primary"
            text
            @click="hideDialog"
          >
            Close
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
</template>

<script>
export default {
  computed: {
    getDialog() {
      return this.$store.getters.getDialog
    }
  },
  methods : {
    hideDialog() {
      this.$store.dispatch('setDialog', {
        isShow: false,
        title: '',
        message: ''
      })
    }
  }
  
}
</script>

<style>

</style>