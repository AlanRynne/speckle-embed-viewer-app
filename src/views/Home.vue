<template>
  <v-container fill-height class="home flex-column justify-center align-center primary--text">
    <h1 class="mb-4">Speckle Embed Viewer App</h1>

    <p> Embed any public stream (or stream you have access to) by going to the url</p>
    <p class="text-caption">{{baseUrl}}/streams/<b>STREAM_ID</b></p>
    <p>or access the latest branch commit on</p>
    <p class="text-caption">{{baseUrl}}/streams/<b>STREAM_ID</b>/branch/<b>BRANCH_ID</b></p>

  </v-container>
</template>

<script>

  export default {
    name: 'Home',
    data(){
      return {
        baseUrl: window.location.origin
      }
    }
  }
</script>
