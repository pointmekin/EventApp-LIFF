<template>
  <div>
    <v-app-bar  color="primary accent-4" dense dark flat>
      <v-toolbar-title class="toolbar-title">Register</v-toolbar-title>
    </v-app-bar>
    <v-container class="pt-0 pb-0">
      <v-row>
        <v-col cols="12 set-padding">
          <div class="text-center mt-15">
            <img width=236 src="https://i.dlpng.com/static/png/7254940_preview.png" alt="done image">
            <h1 class="text-title mt-5">Thank you!</h1>
            <p class="mt-7 mx-12">Thanks for completing this survey. Your feedback will help us improve the next event.</p>
          </div>
          <v-btn rounded outlined color="primary" class="w-100 my-btn mt-5 bold" @click="close">Close</v-btn>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
export default {
  methods: {
    close() {
      liff.closeWindow();
    }
  },
  data() {
    return {
      name: this.$store.getters.getRegister.firstname
    }
  }
}
</script>
