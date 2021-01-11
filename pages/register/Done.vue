<template>
  <div>
    <v-app-bar  color="primary accent-4" dense dark flat>
      <v-toolbar-title class="toolbar-title">Register</v-toolbar-title>
    </v-app-bar>
    <v-container class="pt-0 pb-0">
      <v-row>
        <v-col cols="12 set-padding">
          <div class="text-center mt-5">
            <img width=236 src="https://mir-s3-cdn-cf.behance.net/project_modules/2800_opt_1/2390cd76271827.5c6483c009459.png" alt="done image">
            <h1 class="text-title mt-5">Welcome, {{name}}</h1>
            <p class="mt-7">Welcome to the event! <br/> Tons of workshops are waiting for you. <br/>Hope you will have a great time!</p>
          </div>
          <v-btn rounded color="primary" class="w-100 my-btn mt-10 bold" @click="workshop">Workshop</v-btn>
          <v-btn rounded color="primary" class="w-100 my-btn mt-5 bold" @click="rewards">Rewards</v-btn>
          <v-btn rounded color="primary" class="w-100 my-btn mt-5 bold" @click="survey">Survey</v-btn>
          <v-btn rounded outlined color="primary" class="w-100 my-btn mt-5 bold" @click="close">Close</v-btn>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
export default {
  methods: {
    workshop() {
      this.$router.push('workshop')
    },
    rewards(){
      this.$router.push('reward')
    },
    close(){
      liff.closeWindow();
    },
    survey() {
      this.$router.push('reward')
    }

  },
  data() {
    return {
      name: this.$store.getters.getRegister.firstname
    }
  }
}
</script>
