<template>
  <div class="fill-width error d-flex flex-column justify-center align-center white--text pa-0 ma-0">
    <v-icon x-large>mdi-smiley-sad-outline</v-icon>
    <h1>Ouch! Something went wrong...</h1>

    <p>{{ message }}</p>
  </div>
</template>

<script>
export default {
  name: "Error",
  props: ["message"]
}
</script>

<style scoped>
.fill-width {
  width: 100% !important;
  height: 100% !important;
}
</style>