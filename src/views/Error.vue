<template>
  <v-container fill-height class="error flex-column justify-center align-center white--text">
    <v-icon x-large>mdi-smiley-sad-outline</v-icon>
    <h1>Ouch! Something went wrong...</h1>

    <p>{{ message }}</p>
  </v-container>
</template>

<script>
export default {
  name: "Error",
  props: ["message"]
}
</script>

<style scoped>

</style>