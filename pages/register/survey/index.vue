<template>
  <div>
    <v-app-bar color="primary accent-4" dense dark flat>
      <v-toolbar-title class="toolbar-title pt-2">Survey</v-toolbar-title>
    </v-app-bar>
    <v-container>
      <v-row>
        <v-col cols="12">
          <div class="mt-8 text-primary text-title text-center">Step 1 of 3</div>
        </v-col>
      </v-row>
      <v-row justify="center">
        <v-col cols="10">
          <div>
            <h2 class="text-center mt-1 question">How would you rate <br/> the event overall?</h2>
          </div>
        </v-col>
        <v-col cols="10">
          <v-slider
            class="slider"
            v-model="form.survey1"
            :max=10
            :min=0
            thumb-label
          ></v-slider>
          <v-btn rounded color="primary" class="w-100 my-btn mt-100 my-btn" @click="next"> Next </v-btn>
        </v-col>
      </v-row>
      
    </v-container>
    
  </div>
</template>

<script>
  const axios = require('axios');
  export default {
    data () {
      return {
        form:{
          survey1: this.$store.getters.getSurvey.survey1
        },
      }
    },
    methods: {
      next() {
        this.$store.dispatch("setSurvey", this.form)
        axios.patch("https://nuxt-event-app-default-rtdb.firebaseio.com/survey/line:0001.json", {survey1: this.form.survey1})
        .then((res)=> {
          this.$router.push('/survey/step2')
        })
        .catch(err=> {console.log(err)})
        console.log("register")

      }
    }
  }
</script>

<style lang="scss" scoped>
  .slider {
    margin-top: 90px;
    ::v-deep {
      .v-slider__thumb-label{
        font-size: 20px;
        width: 45px !important;
        height: 45px !important;
      }
      .v-slider__thumb {
        height: 15px !important;
        width: 15px !important;
      }
      .v-slider__track-background {
        background:rgba($color:#000, $alpha: 0.3) !important;
      }
      .v-slider--horizontal .v-slider__track-container {
        height: 4px;
      }
    }
  }
  .my-btn {
    margin-top: 100px;
  }
</style>