<template>
  <div>
    <v-app-bar color="primary accent-4" dense dark flat>
      <v-toolbar-title class="toolbar-title pt-2">Agenda</v-toolbar-title>
    </v-app-bar>
    <v-sheet
      color="white"
      elevation="6"
    >
      <v-tabs
        grow
        v-model="tab"
        background-color= "#ffffff"
        light
      >
        <v-tab
          v-for="item in agenda"
          :key="item.date"
        >
          <p>{{ item.date }}</p>
        </v-tab>
      </v-tabs>
    </v-sheet>
    <v-tabs-items v-model="tab">
      <v-tab-item
        v-for="item in agenda"
        :key="item.date"
      >
        <v-container class="pt-0">
          <v-row>
            <v-col cols="12" class="px-0 py-0">
              <v-col cols="12" v-for="session in item.sessions" :key="session.title">
                <v-card class="agenda-card" :class="session.type == 'set' ? 'card-set' : '' ">
                  <v-card-text v-if="session.type !== 'set' ">
                    <p class="time pb-0">{{session.time}} </p>
                    <v-row>
                      <v-col cols="4" class="text-center py-0">
                        <div class="">
                          <img :src="session.image" alt="" class="agenda-image  mt-2">
                        </div>
                      </v-col>
                      <v-col cols="8" class="py-0">
                        <div>
                          <v-card-title class="px-0 pt-0 pb-3 pt-2 text-primary font-weight-bold">{{session.title}}</v-card-title>
                          <v-card-subtitle class="px-0 py-0">{{session.speaker}}</v-card-subtitle>
                          <p class="px-0 py-0">{{session.duringTime}}</p>
                        </div>
                      </v-col>
                    </v-row>
                  </v-card-text>
                  <div v-else>
                    <v-card-text v-for="sessionSet in session.sessionSet" :key="sessionSet.title">
                    <p class="time pb-0">{{session.time}}</p>
                    <v-row>
                      <v-col cols="4" class="text-center py-0">
                        <div class="">
                          <img :src="sessionSet.image" alt="" class="agenda-image mt-2">
                        </div>
                      </v-col>
                      <v-col cols="8" class="py-0">
                        <div>
                          <v-card-title class="px-0 pt-0 pb-3 pt-2 text-primary font-weight-bold">{{sessionSet.title}}</v-card-title>
                          <v-card-subtitle class="px-0 py-0">{{sessionSet.speaker}}</v-card-subtitle>
                          <p class="px-0 py-0">{{sessionSet.duringTime}}</p>
                        </div>
                      </v-col>
                    </v-row>
                    <hr v-if="sessionSet != session.sessionSet[session.sessionSet.length -1]"/>
                  </v-card-text>
                  </div>
                </v-card>
                
                  
              </v-col>
              
            </v-col>
          </v-row>
        </v-container>
        
      </v-tab-item>
    </v-tabs-items>
  </div>

</template>

<script>
  export default {
    data () {
      return {
        tab: null,
        items: [
          { tab: '20 March 2020', content: 'Tab 1 Content' },
          { tab: '21 March 2020', content: 'Tab 2 Content' },
        ],
      }
    },
    asyncData({store}) {
      // api
      return {
        agenda: store.getters.getAgenda
      }
    }
  }
</script>

<style lang="scss" scoped>
  .v-tabs{
    margin-bottom: 15px;
  }
  .v-tab p {
    margin: 0px;
    padding: 0px;
  }
  .v-tab {
    color: #000;
    background-color: #EFEFEF;
    margin: 0;
    padding: 0;
    &.v-tab--active p{
      color: #1A56BE !important;
      font-weight: bold;
    }
    &+.v-tab {
      border-left: #BDBDBD solid 1px;
    }
  }
  .agenda-card {
    .time{
      color: #1A56BE;
      font-weight: bold;
    }
    .agenda-image {
      width: 50px;
      height: 50px;
      object-fit: cover;
      border-radius: 50%;
    }
    &+.agenda-card{
      margin-top: 15px;

    }
    &.card-set {
      border-left: 3px solid #1A56BE;
      .v-card__text+.v-card__text {
        padding-top: 0 !important;
      }
    }
    hr {
      width: 90%;
      margin: 0 auto;
      border: 0;
      background-color: #BDBDBD;
      height: 1px;
      margin-top: 10px;
    }
  }
  
</style>