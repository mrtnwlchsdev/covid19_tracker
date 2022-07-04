<template>
  <main v-if="!loading">
    <DataTitle 
      :text="title"
      :dataDate="dataDate"/>
    <DataBoxes 
      :stats="stats"
      :text="title"
      :countries="countries"/>
    <CountrySelect 
      :countries="countries"
      @selected-country="selectedCountry" />
  </main>

  <main 
    class="flex flex-col align-center justify-center text-center" v-else>
    <div 
      class="text-gray-500 text-3xl mt-10 mb-6">
      Loading
    </div>
    <div class="lds-hourglass"></div>
  </main>
</template>

<script>
// @ is an alias to /src
import DataTitle from '@/components/DataTitle.vue'
import DataBoxes from '@/components/DataBoxes.vue'
import CountrySelect from '@/components/CountrySelect.vue'

export default {
  name: 'HomeView',
  components: {
    DataTitle,
    DataBoxes,
    CountrySelect
  },
  data() {
    return {
      loading: true,
      title: 'Global',
      dataDate: '',
      stats: {},
      countries: [],
    }
  },
  methods: {
    async fetchCovidData() {
      const response = await fetch('https://api.covid19api.com/summary')
      const data = await response.json()
      return data
    },
    selectedCountry(select,index) {
      this.title = select
      this.stats = this.countries[index]
    }
  },
  async created() {
    const data = await this.fetchCovidData()

    this.dataDate = data.Date
    this.stats = data.Global
    this.countries = data.Countries
    this.loading = false
  }
}
</script>

<style scoped>
.lds-hourglass {
  display: inline-block;
  position: relative;
  width: 80px;
  height: 80px;
  margin: auto;
}
.lds-hourglass:after {
  content: " ";
  display: block;
  border-radius: 50%;
  width: 0;
  height: 0;
  margin: 8px;
  box-sizing: border-box;
  border: 32px solid #2044ac;
  border-color: #2044ac transparent #2044ac transparent;
  animation: lds-hourglass 1.2s infinite;
}
@keyframes lds-hourglass {
  0% {
    transform: rotate(0);
    animation-timing-function: cubic-bezier(0.55, 0.055, 0.675, 0.19);
  }
  50% {
    transform: rotate(900deg);
    animation-timing-function: cubic-bezier(0.215, 0.61, 0.355, 1);
  }
  100% {
    transform: rotate(1800deg);
  }
}
</style>