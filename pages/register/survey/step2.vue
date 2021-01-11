<template>
  <div>
    <v-app-bar color="primary accent-4" dense dark flat>
      <v-toolbar-title class="toolbar-title pt-2">Survey</v-toolbar-title>
    </v-app-bar>
    <v-container>
      <v-row>
        <v-col cols="12">
          <div class="mt-8 text-primary text-title text-center">Step 2 of 3</div>
        </v-col>
      </v-row>
      <v-row justify="center">
        <v-col cols="10">
          <div>
            <h2 class="text-center mt-1 question">What parts of the event did you enjoy?</h2>
          </div>
        </v-col>
        <v-col cols="10">
          <template>
            <v-container fluid>
              <v-textarea
                clearable
                clear-icon="mdi-close-circle"
                label="Comments"
                v-model="form.survey2"
                :value="form.survey2"
              ></v-textarea>
            </v-container>
          </template>

          <v-btn rounded color="primary" class="w-100 my-btn mt-100 my-btn" @click="next"> Next </v-btn>
          <div class="w-100 text-center my-btn text-primary" @click="back()"> Back </div>
        </v-col>
      </v-row>
      
    </v-container>
    
  </div>
</template>

<script>
const axios = require('axios');
export default {
  data() {
    return {
      form:{
        survey2: this.$store.getters.getSurvey.survey2
      }
    }
  },
  methods: {
    next() {
      this.$store.dispatch("setSurvey", this.form)
      axios.patch("https://nuxt-event-app-default-rtdb.firebaseio.com/survey/line:0001.json", {survey2: this.form.survey2})
      .then((res)=> {
        this.$router.push('/survey/step3')
      })
      .catch(err=> {console.log(err)})
      console.log("register")
    },
    back() {
      this.$store.dispatch("setSurvey", this.form)
      this.$router.push('/survey')
    }
  }

}
</script>

<style>

</style>