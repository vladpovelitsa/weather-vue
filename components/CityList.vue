<template>
  <div id="CityList">
    <v-row>
      <v-col cols="12" md="6" lg="4" v-if="lat && lon">
        <CityCard :cityLon="lon" :cityLat="lat" @removingCity="removeCity">
          <template v-slot:home>
            <div class="d-flex blue--text mr-4" small>
              <v-icon color="blue"> mdi-home </v-icon>
              Текущее местоположение:
            </div>
          </template>
        </CityCard>
      </v-col>

      <v-col
        cols="12"
        md="6"
        align="start"
        lg="4"
        v-for="(city, key) in this.cities"
        v-bind:key="key"
      >
        <CityCard :cityName="city" :id="key" @removingCity="removeCity">
          <template v-slot:home>
            <div class="d-flex blue--text mr-4" small>
              <v-icon color="blue" class="mr-2"> mdi-city </v-icon>
              Другой город:
            </div>
          </template>
        </CityCard>
      </v-col>
    </v-row>
  </div>
</template>

<script>
import CityCard from '~/components/CityCard.vue'

export default {
  name: 'CityList',
  components: {
    CityCard,
  },
  props: {
    cities: {
      required: true,
    },
    lat: {
      required: false,
    },
    lon: {
      required: false,
    },
  },
  data() {
    return {}
  },
  methods: {
    removeCity(id) {
      this.$emit('deleteCity', id)
      this.cities = this.cities
    },
  },
}
</script>

<style scoped lang="scss"></style>
