<template>
  <div id="CityCard">
    <v-skeleton-loader v-if="loading" type="image, article"></v-skeleton-loader>
    <v-card
      :loading="loading"
      class="mx-auto"
      height="100%"
      max-width="100%"
      v-else-if="weather.cod == 200 && !loading"
    >
      <template slot="progress">
        <v-progress-linear
          color="blue"
          height="5"
          indeterminate
        ></v-progress-linear>
      </template>

      <img
        height="200"
        width="100%"
        src="../assets/img/card_bg.jpg"
        class="card_cover"
      >

      <v-card-title class="flex flex-column align-start">
        <slot name="home"></slot>
        <slot name='another'></slot>
        <div>
         {{ weather.name }} 
        </div>
      </v-card-title>
        
      </v-card-text>
      <v-card-title>
        <div class="d-flex align-center">
          <div>
            {{ weather.weather[0].description }}
          </div>
          <img
            :title="weather.weather[0].description"
            :src="`http://openweathermap.org/img/wn/${weather.weather[0].icon}.png`"
            :alt="weather.weather[0].description"
          />
        </div>
      </v-card-title>
      <v-card-text>
        <div>
          Температура, °C: <b>{{ weather.main.temp }}</b>
        </div>
        <div>
          Чувствуется как, °C: <b>{{ weather.main.feels_like }}</b>
        </div>
        <div>
          Влажность, %: <b>{{ weather.main.humidity }}</b>
        </div>
        <div>
          Давление, мм: <b>{{ weather.main.pressure }}</b>
        </div>
        <div>
          Ветер, м/сек: <b>{{ weather.wind.speed }}</b>
        </div>
      </v-card-text>

      <v-card-actions >
        <v-btn x-small dark color="red" @click="removeCity()" :disabled='!cityName'>
          <v-icon x-small> mdi-delete </v-icon>
        </v-btn>
      </v-card-actions>
    </v-card>
    <v-card color="red" dark v-else height="100%">
      <v-card-title> Город не найден </v-card-title>
    </v-card>
  </div>
</template>

<script>
export default {
  name: 'CityCard',
  props: {
    cityName: {
      required: false,
    },
    id: {
      required: false,
    },
    cityLon: {
      required: false,
    },
    cityLat: {
      required: false,
    },
  },

  data() {
    return {
      loading: true,
      apiKey: '967c67c047f4249b6c55f6803ab1324d',
      workingKey: '439d4b804bc8187953eb36d2a8c26a02',
      url: 'https://api.openweathermap.org/data/2.5/weather',
      weather: {
        main: {},
        wind: {},
      },
    }
  },
  methods: {
    async fetchWeather() {
      const params = {
        lat: this.cityLat,
        lon: this.cityLon,
        city: this.cityName,
        url: this.url,
        apiKey: this.apiKey,
      }

      const request =
        params.lat && params.lon != undefined
          ? `${params.url}?lat=${params.lat}&lon=${params.lon}&units=metric&appid=${params.apiKey}&lang=ru`
          : `${params.url}?q=${params.city}&units=metric&appid=${params.apiKey}&lang=ru`

      return await fetch(request)
        .then((res) => {
          return res.json()
        })
        .then((data) => {
          this.loading = false

          if (data.cod !== 200) {
            let timeout = setTimeout(() => {
              this.removeCity()
            }, 5000)
          } else {
            this.weather = data
          }
        })
        .catch((er) => {
          console.log(er)
        })
    },
    removeCity() {
      this.$emit('removingCity', this.id)
    },
  },
  created() {
    this.fetchWeather()
  },
}
</script>

<style scoped lang="scss">
    .card_cover {
        object-fit: cover;
        object-position: center;
    }
</style>
