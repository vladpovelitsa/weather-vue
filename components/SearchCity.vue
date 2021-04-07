<template>
  <div id="SearchCity">
    <v-row>
      <v-col cols="12" align="center">
        <h2 class="search_city__title" center>Погода в твоем городе</h2>
      </v-col>
    </v-row>
    <v-form
      v-model="validSearchCity"
      ref="searchCity"
      class="search_city"
      label="test"
      @submit.prevent="sendNewCity"
    >
      <v-row align="center" justify="center">
        <v-col cols="12" md="6">
          <v-text-field
            label="Укажи город"
            required
            v-model="newCity"
            :rules="[(v) => !!v || 'Нужен город']"
          ></v-text-field>
        </v-col>
        <v-col cols="12" md="3">
          <v-btn color="blue darken-4" width="100%" dark type="sumbit">
            Узнать погоду
          </v-btn>
        </v-col>
      </v-row>
    </v-form>
  </div>
</template>

<script>
export default {
  name: 'SearchCity',
  data() {
    return {
      validSearchCity: false,
      newCity: '',
    }
  },
  methods: {
    sendNewCity() {
      if (this.validSearchCity) {
        this.$emit('addCity', this.newCity)
        this.$refs.searchCity.reset()
      } else {
        this.$refs.searchCity.validate()
      }
    },
  },
}
</script>

<style scoped lang="scss">
.search_city {
  padding: 20px 30px;
  margin-bottom: 30px;
}
.search_city__title {
  font-size: 36px;
  margin-bottom: 20px;
  color: #993718;
}
</style>
