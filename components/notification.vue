<template>
  <div>
    <div class="text-center mb-4">
      <v-btn color="primary" @click="alert = !alert">Disclaimer</v-btn>
    </div>
    <v-alert
      :value="alert"
      color="orange"
      dark
      border="left"
      icon="mdi-message-text"
      transition="scale-transition"
      elevation="5"
    >
      Weather data come from API.
      Data will be displayed after entering the city name.
    </v-alert>
  </div>
</template>


<script>
export default {
  data() {
    return {
      alert: true,
    }
  },
}
</script>
