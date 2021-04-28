<template>
  <q-page class="flex column">
    <div class="col q-pt-lg q-px-md">
      <q-input
        v-model="search"
        @keyup.enter="getWeatherBySearch"
        placeholder="Search"
        dark
        borderless
      > 
        <template v-slot:before>
          <!-- <q-icon
            @click="getLocation"
            name="my_location"/> -->
        </template>

        <template v-slot:append>
          <q-btn
          @click="getWeatherBySearch" 
          round 
          dense 
          flat 
          icon="search" />
        </template>
      </q-input>
    </div>

    <template v-if="weatherData">
      <div class="col text-white text-center">
        <div class="text-h4 text-weight-light">
          {{weatherData.name}}
        </div>
        <div class="text-h6 text-weight-light">
          {{weatherData.weather[0].main}}
        </div>
        <div class="text-h1 text-weight-thin q-my-lg relative-position temp">
          {{Math.round(weatherData.main.temp)}}
          <span class="text-h3 relative-position degree">&deg;</span>   
        </div>
      </div>
      <div class="col text-center ">
      <img :src="`${imgUrl}${ weatherData.weather[0].icon }.svg`" alt="" class="w-img">
      </div>
    </template>
    <template v-else >
      <!-- <div class="col column text-center text-white">
        <div class="col text-h2 text-weight-thin title">
          Quasar Weather
        </div>
        <q-btn
          @load="getWeatherByCoords"
          class="col" 
          flat>
          <q-icon left size="3em" name="my_location" />
          <div>Find My Location</div>
        </q-btn>
      </div>  -->
    </template>

    <div class="col skyline"></div>
  </q-page >
</template>

<script>
export default {
  name: 'pageIndex',
  data() {
    return {
      search:'',
      weatherData: null,
      lat: null,
      lon: null,
      apiUrl: 'https://api.openweathermap.org/data/2.5/weather',
      apiKey: '33c2e612a18661498a48130bd6299e08',
      imgUrl:'https://raw.githubusercontent.com/richwalton/owd_icons/master/'
    }
  },
  methods: {
    // getLocation() {
    //   navigator.geolocation.getCurrentPosition(position => {
    //     console.log('position: ', position)
    //     this.lat = position.coords.latitude
    //     this.lon = position.coords.longitude
    //     this.getWeatherByCoords()
    //   })
    // },
    getWeatherByCoords() {
      this.$axios(`${ this.apiUrl }?q=Tucson,us&appid=${ this.apiKey }&units=imperial`).then(response => {
        this.weatherData = response.data
      })  
    },
    getWeatherBySearch() {
      console.log('getWeatherBySearch')
      this.$axios(`${ this.apiUrl }?q=${ this.search }&appid=${ this.apiKey }&units=imperial`).then(response => {
        this.weatherData = response.data
      })  
    }
  },
  beforeMount(){
    this.getWeatherByCoords()
  }
}
</script>
<style lang="sass">
  .q-field
    font-size: 24px
  .q-page
    background: linear-gradient(to top, #0575e6, #00356af0, #021b79)
  .degree
    top: -40px
  .title
    line-height:1.15em  
  .skyline
    flex: 0 0 100px
    background: url(../assets/tucson-skyline3.png)
    background-size: cover
    background-position: center bottom 
    background-repeat: no-repeat 
  .w-img
    max-width:65px
    height: 75px  
    margin:10px 15px 20px 
  .temp
    letter-spacing: 0em
    margin-right: -12px


</style>    