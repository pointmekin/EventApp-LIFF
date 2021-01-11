<template>
  <div>
    <v-app-bar color="primary accent-4" dense dark flat>
      <v-toolbar-title class="toolbar-title">Register Workshop</v-toolbar-title>
    </v-app-bar>
    <v-container class="pt-0 pb-0">
      <v-row>
        <v-col cols="12">
          <div class="mt-3 text-primary text-title text-center">
            {{list.date}}
          </div>
        </v-col>
      </v-row>
      <v-row>
        <v-col cols="12">
          <Card 
          v-for="item in list.sessions"
          :session="item"
          :key="item.id"
          :active="selectedWorkshop.includes(item.id)"
          v-on:moreDetails="moreDetails(item)"
          v-on:chooseWorkshop="chooseWorkshop(item)"
          />
          <div class="set-padding">
            <v-btn
              rounded
              color="primary"
              class="w-100 my-btn mt-3"
              @click="next"
            >
              Next
            </v-btn>
            <div v-if="index > 0" class="w-100 text-center my-btn text-primary mt-3" @click="back()"> Back </div>
          </div>
        </v-col>
      </v-row>
      <v-dialog v-model="isShowDialog" persistent max-width="290">
        <v-card>
          <v-img
            class="white--text align-end"
            height="200px"
            :src= "dialog.image"
          >
          </v-img>
          <v-card-title class="headline">{{dialog.title}} </v-card-title>
          <v-card-text>
            <p>{{dialog.time}} </p>
            <p>{{dialog.place}} </p>
            <p class="detail">
              {{dialog.detail}} 
            </p>
            <p>Speaker(s):<br/> <span v-html="dialog.speakers"></span></p>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn
              class="text-primary"
              color="green darken-1"
              text
              @click="isShowDialog = false"
            >
              OK
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-container>
  </div>
</template>

<script>
import Card from "../../../components/Card";
const axios = require('axios');
export default {
  components: {
    Card,
  },
  data() {
    return {
      isShowDialog: false,
      index: 0,
      dialog: {
        title: '',
        time:'',
        place: '',
        image: '',
        detail: '',
        speaker:''
      },
      selectedWorkshop: [],
      list: [],
      workshops: this.$store.getters.getWorkshop
    }
  },
  mounted() {
    // api
    this.list = this.workshops[this.index]
  },
  methods: {
    back() {
      if (this.index === 0) {
        // api to save workshop
      } else {
        this.index = this.index -1
        this.list = this.workshops[this.index]
      }
    },
    next() {
      if (this.index === this.workshops.length -1) {
        this.$store.dispatch('setRegister', this.form)
        axios.patch(`https://nuxt-event-app-default-rtdb.firebaseio.com/workshops/${this.$store.getters.getLine.userId}.json`, {...this.selectedWorkshop})
        .then((res)=> {
          console.log(res)
          this.$router.push('done')
        })
        .catch(err=> {alert(err.toString())})
      } else {
        this.index = this.index + 1
        this.list = this.workshops[this.index]
      }
    },
    moreDetails(item) {
      this.isShowDialog = true
      this.dialog = item
    },
    chooseWorkshop(item) {
      const listId = this.list.sessions.map(session => session.id)
      this.selectedWorkshop = this.selectedWorkshop.filter(session=> !listId.includes(session))
      this.selectedWorkshop.push(item.id)
    },
  },
};
</script>

<style lang="scss" scoped>
.more-details {
  font-size: 12px;
  color: #1a56be;
  background-color: #ffffff;
  padding: 10px 20px;
  border-radius: 25px;
  font-weight: bold;
  display: inline;
  position: absolute;
  bottom: 11px;
  right: 11px;
}
.v-card_text {
  padding: 0px !important;
  padding-bottom: 5px;
}
h1 {
  font-size: 24px;
}

.circle {
  border-radius: 50%;
  height: 32px !important;
  width: 32px !important;
  min-width: auto !important;
  background: rgba($color: #1a56be, $alpha: 0.3) !important;
  color: #fff;
  align-self: center;
}
.v-card__subtitle,
.v-card__text,
.v-card__title {
  padding: 0;
}
.card-content {
  display: flex;
  justify-content: space-between;
  padding: 15px;
}

.v-card_subtitle {
  margin-top: 10px !important;
}
.group {
  padding: 0;
}
.v-card + .v-card {
  margin-top: 30px;
}
.v-card.active {
  .circle {
    background: rgba($color: #1a56be, $alpha: 1) !important;
  }
  .v-image {
    &::before {
      content: "Selected";
      height: 100%;
      width: 100%;
      font-weight: bold;
      font-size: 24px;
      display: flex;
      align-items: center;
      justify-content: center;
      background: #1a56be;
    }
  }
}

.v-dialog {
  .v-card.v-card__text {
    padding: 0 24px;
  }
  p {
    margin-bottom: 0px;
  }
  .v-card__title.headline {
    font-size: 20px !important;
  }
  .detail {
    margin: 10px 0;
  }
}
</style>