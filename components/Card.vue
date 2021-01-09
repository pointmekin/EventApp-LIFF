<template>
  <v-card class="mx-auto" :class="active === true ? 'active' : ''" max-width="380">
    <v-img
      class="white--text align-end"
      height="200px"
      src="https://cdn.vuetifyjs.com/images/cards/docks.jpg"
    >
    <div class="more-details" @click="$emit('moreDetails')">More Details</div>
    </v-img>
    <div class="card-content">
      <div class="group">
        <v-card-text class="text--primary">
        <h1>{{ session.title }}</h1>
        </v-card-text>
        <v-card-subtitle class="pt-3">{{ session.speaker}} </v-card-subtitle>
      </div>
      <v-btn class="circle" @click="$emit('chooseWorkshop')"><v-icon>check</v-icon></v-btn>
    </div>       
  </v-card>
</template>

<script>
export default {
  props: ['active', 'session']

}
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
</style>