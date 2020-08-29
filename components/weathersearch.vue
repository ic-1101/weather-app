<template>
  <v-container :class="typeof weather.main != 'undefined'">
<!--  Disclaimer  -->
    <div>
      <div class="text-center mb-4">
        <v-btn
          color="orange"
          @click="alert = !alert"
        >
          Disclaimer
        </v-btn>
      </div>
      <v-alert
        :value="alert"
        color="red"
        dark
        border="left"
        icon="mdi-message"
        transition="scale-transition"
        class="text-justify"
      >
        Weather data come from real-time API. So data should be accurate. If not then API are not
        responsible for this error, but you are! Because of climate changes, maybe you are feeling cold or
        warm right now, which is not matching with API search data, then you are the responsible for it -
        because you are damaging your environment. So first change yourself - then report about API service.
        Stay safe - Stay home or suffer.
      </v-alert>
    </div>
<!--  End Disclaimer  -->
    <v-row>
      <v-col
        cols="12"
        md="6"
      >
        <v-card
          class="pa-2"
          outlined
          tile
          elevation="5"
        >
          <v-form
            @submit.prevent="getWeatherData"
            ref="form"
            v-model="valid"
            lazy-validation
          >
            <v-text-field
              class="mt-2"
              v-model="city"
              :rules="nameRules"
              label="City"
              placeholder="Enter city name here . . ."
              outlined
              filled
              required
              append-icon="mdi-map-marker"
              hint="Example, Dhaka or type your city"
              @keypress="resetValidation"
            ></v-text-field>

            <v-btn
              color="error"
              class="mr-2 mb-2"
              @click="reset"
            >
              Reset Form
            </v-btn>

            <v-btn
              color="warning"
              @click="resetValidation"
              class="mb-2"
            >
              Reset Validation
            </v-btn>
          </v-form>
        </v-card>
      </v-col>
      <v-col
        cols="12"
        md="6"
        v-if="typeof weather.main != 'undefined'"
      >
        <v-card
          class="pa-2"
          outlined
          elevation="5"
          tile
        >
          <v-img
            :src="icon"
            width="50"
            height="50"
            class="rounded-circle ml-4"
          ></v-img>
          <v-list-item>
            <v-list-item-content>
              <v-list-item-title class="headline mb-1">
                {{ weather.name }}, {{ weather.sys.country }}
              </v-list-item-title>
              <v-list-item-subtitle>
                {{ dateBuilder() }}, {{ weather.weather[0].description }}. {{ weather.weather[0].main }}.
              </v-list-item-subtitle>
              <div class="overline mb-0">{{ Math.round(weather.main.temp) }}&deg;C - Feelslike: {{ Math.round(weather.main.feels_like) }}&deg;C </div>
              <v-divider></v-divider>
              <div class="overline mb-0 text-capitalize">Minimum Temp: {{ Math.round(weather.main.temp_min) }}&deg;C - Maximum Temp: {{ Math.round(weather.main.temp_max) }}&deg;C</div>
              <div class="overline mb-0 text-capitalize">Wind Speed: {{ weather.wind.speed }}(m/s)</div>
              <div class="overline mb-0 text-capitalize">Humidity: {{ weather.main.humidity }}%</div>
              <div class="overline mb-0 text-capitalize">Pressure: {{ weather.main.pressure }}(hpa)</div>
              <div class="overline mb-0 text-capitalize">Visibility: {{ weather.visibility/1000 }}(km)</div>
            </v-list-item-content>
          </v-list-item>
        </v-card>
      </v-col>
    </v-row>

  </v-container>
</template>


<script>
export default {
  data: () => ({
    valid: true,
    city: '',
    country: '',
    nameRules: [
      v => !!v || 'City name is required',
    ],
    alert: true,
    weather: {}

  }),

  created() {
    this.getWeatherData()
  },

  methods: {
    reset () {
      this.$refs.form.reset()
    },
    resetValidation () {
      this.$refs.form.resetValidation()
    },
    getWeatherData() {
      const proxyurl = 'https://cors-anywhere.herokuapp.com/'
      this.$axios
        .$get(
          `${proxyurl}https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=${process.env.APPID}`
        )
        .then(res => (this.weather = res))
      this.city = '';
    },
    dateBuilder () {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    }
  },
  computed: {
    icon() {
      return this.weather.weather
        ? `https://openweathermap.org/img/wn/${this.weather.weather[0].icon}.png`
        : ''
    },
    temp_max() {
      return this.weather.main
        ? Math.round(this.weather.main.temp_max - 273)
        : ''
    },
    temp_min() {
      return this.weather.main
        ? Math.round(this.weather.main.temp_min - 273)
        : ''
    }
  }
}
</script>
