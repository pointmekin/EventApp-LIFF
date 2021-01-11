<template>
  <div>
    <v-app-bar color="primary accent-4" dense dark flat>
      <v-toolbar-title class="toolbar-title pt-2">Survey</v-toolbar-title>
    </v-app-bar>
    <v-container>
      <v-row>
        <v-col cols="12">
          <div class="mt-8 text-primary text-title text-center">Step 3 of 3</div>
        </v-col>
      </v-row>
      <v-row justify="center">
        <v-col cols="10">
          <div>
            <h2 class="text-center mt-1 question">Finally, would you recommend our event to a friend?</h2>
          </div>
        </v-col>
        <v-col cols="10">
          <div class="select" :class="form.survey3 == 1 ? 'active' : '' " @click="form.survey3 = 1">
            <h1>Of course!</h1>
            <p>Definitely. I'll join the next event.</p>
            <v-icon>check</v-icon>
          </div>
          <div class="select" :class="form.survey3 == 2 ? 'active' : '' " @click="form.survey3 = 2">
            <h1>Not sure</h1>
            <p>Let me consider it again next time.</p>
            <v-icon>check</v-icon>
          </div>
          <div class="select" :class="form.survey3 == 3 ? 'active' : '' " @click="form.survey3 = 3">
            <h1>No, thanks</h1>
            <p>It's not my type.</p>
            <v-icon>check</v-icon>
          </div>
        </v-col>
        <v-col cols="11">
          <v-btn rounded color="primary" class="w-100 my-btn mt-50 my-btn" @click="submit"> Submit </v-btn>
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
      form: {
        survey3: 1
      }
    }
  },
  methods: {
    back() {
      this.$store.dispatch("setSurvey", this.form)
      this.$router.push('/survey/step2')
    },
    submit() {
      this.$store.dispatch("setSurvey", this.form)
      axios.patch("https://nuxt-event-app-default-rtdb.firebaseio.com/survey/line:0001.json", {survey3: this.form.survey3})
      .then((res)=> {
        this.$router.push('/survey/done')
      })
      .catch(err=> {console.log(err)})
    },
  }

}
</script>

<style lang="scss" scoped>
  .question {
    font-size: 20px;
  }
  .select {
    padding-bottom: 25px;
    border-bottom: 1px solid #E6E6E6;
    position: relative;
    color: #707070;
    margin-bottom: 25px;
    
    &.active {
      color: #1A56BE !important;
      .v-icon {
        display: block;
        color: #1A56BE;
      }
    }
    h1 {
      font-size: 20px;
    }
    p {
      font-size: 18px;
      margin-bottom: 0;
    }
    .v-icon {
      position: absolute;
      right: 0;
      bottom: 25px;
      display: none;
    }
  }
</style>