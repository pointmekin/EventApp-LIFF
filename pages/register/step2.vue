<template>
  <div>
    <v-app-bar  color="primary accent-4" dense dark flat>
      <v-toolbar-title class="toolbar-title">Page title</v-toolbar-title>
    </v-app-bar>
    <v-container class="pt-0 pb-0">
      <v-row>
        <v-col cols="12">
          <div class="mt-8 text-primary text-title text-center">Step 2 of 2</div>
        </v-col>
        <v-col cols="12">
          <v-col cols="12" class="text-center text-main pt-1 pb-0">Tell us a bit more </v-col>
          <v-form>
            <v-text-field
              v-model="form.email"
              dense
              type="email"
              label="Email"
              :rules="emailRules"
            ></v-text-field>
            <v-text-field
              v-model="form.phone"
              dense
              label="Phone"
              required
              :rules="phoneRules"
              @keypress="onlyNumber($event, 10)"
            ></v-text-field>
            
            <v-dialog
              ref="dialog"
              v-model="modal"
              :return-value.sync="form.birthday"
              persistent
              width="290px"
            >
              <template v-slot:activator="{ on, attrs }">
                <v-text-field
                  v-model="form.birthday"
                  label="Birthday"
                  prepend-icon="mdi-calendar"
                  readonly
                  v-bind="attrs"
                  v-on="on"
                  class="set-birthday"
                ></v-text-field>
              </template>
              <v-date-picker
                v-model="form.birthday"
                scrollable
                :max="new Date().toISOString().substr(0, 10)"
              >
                <v-spacer></v-spacer>
                <v-btn
                  text
                  color="primary"
                  @click="modal = false"
                >
                  Cancel
                </v-btn>
                <v-btn
                  text
                  color="primary"
                  @click="$refs.dialog.save(form.birthday)"
                >
                  OK
                </v-btn>
              </v-date-picker>
            </v-dialog>

            <v-col cols="12" class="text-center text-main pt-1 pb-0">Work Profile</v-col>
            <v-text-field
              v-model="form.company"
              dense
              label="Company"
              required
            ></v-text-field>
            <v-text-field
              v-model="form.position"
              dense
              label="Position"
              required
            ></v-text-field>
            <v-btn rounded color="primary" class="w-100 my-btn mt-10" @click="register()"> Register </v-btn>
            <div class="w-100 text-center my-btn text-primary" @click="back()"> Back </div>
          </v-form>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
const REGEX_EMAIL = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
const REGEX_PHONE = /^[0]([0-9]{9})*$/
const REGEX_NUMBER = /^[0-9]*$/
const axios = require('axios');
export default {
  data() {
    return {
      form: {
        email: this.$store.getters.getRegister.email,
        phone: this.$store.getters.getRegister.phone,
        birthday: new Date().toISOString().substr(0, 10),
        company: this.$store.getters.getRegister.company,
        position: this.$store.getters.getRegister.position,
      },
      modal: false,
      emailValidated: false,
      emailRules: [value => this.emailValidator(value)],
      phoneValidated: false,
      phoneRules: [value => this.phoneValidator(value)]
    }
  },
  methods: {
    back() {
      this.$router.push('/register')
    },
    register(){
      if (this.validate()) {
        this.$store.dispatch('setRegister', this.form)
        axios.patch(`https://nuxt-event-app-default-rtdb.firebaseio.com/members/${this.$store.getters.getLine.userId}/profile.json`, this.$store.getters.getRegister)
        .then((res)=> {
          this.$router.push('/register/done')
        })
        .catch(err=> {console.log(err)})
        console.log("register")
      }
    },
    emailValidator(value) {
      if (value === "") return "Required"
      if (REGEX_EMAIL.test(value)) {
        this.emailValidated = true
        return true
      } else {
        return ("Email is invalid.")
      }
    },
    phoneValidator(value) {
      if (value === "") return "Required"
      if (REGEX_PHONE.test(value) && value.length === 10) {
        this.phoneValidated = true
        return true
      } else {
        return ("Phone number is invalid.")
      }
    },
    onlyNumber(event, max) {
      if (event.target.value.length === 0) {
        if (event.key != 0) {
          return event.preventDefault()
        }
      }
      if (!REGEX_NUMBER.test(event.key) || event.target.value.length == max){
        return event.preventDefault();
      }
    },
    validate(){
      let validated = true
      let errors = []
      let errorMsg = ''
      const validatorField = [
        'email',
        'phone',
        'company',
        'position'
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
      if (!this.phoneValidated) {
        validated = false
        errors.push(`email is invalid`)
      }
      if (!this.emailValidated) {
        validated = false
        errors.push(`phone is invalid`)
      }
      return validated
    }
  }

}
</script>

<style lang="scss" scoped>
  .v-form {
    padding: 0 10px;
  }
  .set-birthday {
    position: relative;
    ::v-deep .v-input__prepend-outer{
      position: absolute; 
      right: 0
    }
  }

</style>