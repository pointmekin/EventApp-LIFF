<template>
  <div>
    <v-app-bar  color="primary accent-4" dense dark flat>
      <v-toolbar-title class="toolbar-title">Page title</v-toolbar-title>
    </v-app-bar>
    <v-container class="pt-0 pb-0">
      <v-row>
        <v-col cols="12">
          <div class="mt-8 text-primary text-title text-center">Step 1 of 2</div>
        </v-col>
        <v-col cols="12" class="">
          <div class="text-center pb-0 profile-img">
            <img v-if="getLine.pictureUrl == ''" src="https://image.freepik.com/free-vector/cartoon-running-men-running-nature-minimal-flat-illustration_97843-63.jpg" alt="img" width=155>
            <img v-else :src="getLine.pictureUrl" alt="img" width=155/>
          </div>
          <v-col cols="12" class="text-center pt-1 pb-0">
            {{getLine.displayName}}
          </v-col>
          <v-form>
            <v-col cols="12">
              <v-text-field
                v-model="form.firstname"
                dense
                label="First name"
                required
              ></v-text-field>
              <v-text-field
                v-model="form.lastname"
                dense
                label="Last name"
                required
              ></v-text-field>
              <div class="gender-group d-flex mt-5">
                <p m-0 >Gender</p>
                <div class="circle" :class="form.gender==1 ? 'active' : ''" @click="chooseGender(1)">F</div>
                <p>Female</p>
                <div class="circle" :class="form.gender==2 ? 'active' : ''" @click="chooseGender(2)">M</div>
                <p>Male</p>
              </div>
            </v-col>
            <v-btn rounded color="primary" class="w-100 my-btn mt-10" @click="next"> Next </v-btn>
          </v-form>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
const axios = require('axios');
export default {
  mounted() {
    liff.init({
      liffId: '1655563676-nLozlkd2'
    })
    .then(()=> {
      console.log("TEST")
      if (liff.isLoggedIn()) {
        console.log("Logged in")
        liff.getProfile().then((profile)=> {
          this.$store.dispatch('setLine', profile)
          this.isDone()
        })
      } else {
        console.log("Not logged in")
        liff.login();
      }
    })
  },
  computed: {
    getLine() {
      return this.$store.getters.getLine
    }
  },
  data(){
    return ({
      form: {
        firstname: this.$store.getters.getRegister.firstname,
        lastname: this.$store.getters.getRegister.lastname,
        gender: this.$store.getters.getRegister.gender
      }
    })
  },
  methods: {
    isDone() {
      axios.get(`https://nuxt-event-app-default-rtdb.firebaseio.com/members/${this.$store.getters.getLine.userId}/profile.json`).then((res) => {
        if (res.data !== null) {
          this.$router.push('/register/done')
        }
      })
    },
    chooseGender(gender) {
      this.form.gender = gender;
    },
    next(){
      if (this.validate()) {
        this.$store.dispatch('setRegister', this.form)
        this.$router.push('/register/step2')
      }
    },
    validate(){
      let validated = true
      let errors = []
      let errorMsg = ''
      const validatorField = [
        'firstname',
        'lastname'
      ]
      validatorField.forEach((field) => {
        if(this.form[field] == '') {
          validated = false
          errors.push(`${field} cannot by empty`)
        }
      })
      if (!validated){ 
        this.errorMsg = errors.map((error) => error + '<br/>').join('')
        this.dialog = true;
        this.$store.dispatch('setDialog',{
          isShow: true,
          title:'Form has errors',
          message: errors.map((error) => error + '<br/>').join('')
        })
      }
      return validated
    }
  }

}
</script>

<style lang="scss" scoped>
  .toolbar-title {
    width: 100%;
    text-align: center;
    font-weight: bold;
  }
  .v-form {
    padding: 0 5px;
  }
  .circle {
    width: 45px;
    height: 45px;
    padding-top: 10px;
    text-align: center;
    background: #1A56BE;
    color: #ffffff;
    border-radius: 50%;
    position: relative;
    background-color: rgba(0,0,0,0.3);
    margin-right: 10px;
    &.active{
      background:#1A56BE
    }
  }
  p{
    margin-bottom: 0;
    align-self: center;
    margin-right: 15px;
  }
  .profile-img {
    img{
      border-radius: 50%;
    }
  }
</style>