<template>
  <div>
    <SearchCity @addCity="pushingNewCity" />
    <CityList
      :cities="citiesList"
      :lat="latitude"
      :lon="longitude"
      @deleteCity="removingCity"
    />
  </div>
</template>

<script>
import SearchCity from '~/components/SearchCity.vue'
import CityList from '~/components/CityList.vue'

export default {
  components: {
    SearchCity,
    CityList,
  },
  data() {
    return {
      citiesList: [],
      latitude: '',
      longitude: '',
    }
  },
  methods: {
    pushingNewCity(cityName) {
      this.citiesList.push(cityName)
      localStorage.setItem('citiesList', JSON.stringify(this.citiesList))
    },
    removingCity(id) {
      this.citiesList.splice(id, 1)
      localStorage.setItem('citiesList', JSON.stringify(this.citiesList))
    },
    getLocation(position) {
      this.latitude = position.coords.latitude
      this.longitude = position.coords.longitude
    },
  },
  mounted() {
    if (localStorage.citiesList && localStorage.citiesList.length > 0) {
      this.citiesList = JSON.parse(localStorage.getItem('citiesList'))
    }
    navigator.geolocation.getCurrentPosition(this.getLocation)
  },
}
</script>

<style lang="scss">
.v-main__wrap {
  background-image: linear-gradient(120deg, #a1c4fd 0%, #c2e9fb 100%);
}
</style>
