<template>
  <v-app>
    <nuxt></nuxt>
    <Dialog />
  </v-app>
</template>

<script>
import Dialog from '../components/Dialog'
export default {
  components: {
    Dialog
  }
}
</script>

<style lang="scss">
  .v-application {
    font-family: 'Quicksand', sans-serif;
  }
  .v-application .primary {
    background-color: #1A56BE !important;
    border-color: #1A56BE !important;
  }
  .toolbar-title {
    width: 100%;
    text-align: center;
    font-weight: bold;
  }
  .text-primary {
    color : #1A56BE;
    font-weight: bold;
  }
  .text-title {
    font-size:20px;
    font-weight:bold;
  }
  .w-100 {
    width:100%;
  }
  .my-btn {
    font-size: 20px !important;
    padding: 12px 0 !important;
    text-transform: none;
    height: auto !important;
  }
  .text-main{
    font-size: 20px;
  }
  .bold {
    font-weight: bold;
  }
  .set-padding{
    padding: 0 20px;
  }
  .mt-100 {
    margin-top: 100px;
  }
  .mt-50 {
    margin-top: 50px;
  }
</style>