<template>
  <div class="wrapper" v-if="weather">
    <div class="day" :class="{first: i === 0}" :key="i" v-for="(day, i) in nextFiveDays">
      <span class="day-name">{{ getDayName(day) }}</span>

      <img :src="getWeatherIconForDay(day)">
      
      <div class="min-max">
        <span>{{ getDayMin(day) }}</span> 

        <span>{{ getDayMax(day) }}</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      weather: null,
    };
  },

  async mounted() {
    this.weather = await this.fetchWeatherJSON();
  },

  computed: {
    nextFiveDays() {
      return this.weather.daily.slice(0, 5);
    },
  },

  methods: {
    async fetchWeatherJSON() {
      const response = await fetch('https://api.openweathermap.org/data/2.5/onecall?lat=33.448376&lon=-112.074036&appid=f5c255c28d6d0a68e2ae8fda5491bad1&units=imperial');
      const weather = await response.json();

      return weather;
    },

    getWeatherIconForDay(day) {
      return `http://openweathermap.org/img/wn/${day.weather[0].icon}@2x.png`;
    },

    getDayName(day) {
      const weekdays = ["Sun","Mon","Tue","Wed","Thu","Fri","Sat"];
      const date = new Date(day.dt * 1000);

      return weekdays[date.getDay()];
    },

    getDayMin(day){
      return `${Math.round(day.temp.min)}°`;
    },

    getDayMax(day){
      return `${Math.round(day.temp.max)}°`;
    },
  },
};
</script>

<style lang="scss" scoped>
.wrapper {
  border: solid 1px #ddd;
  display: flex;
}

.day {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
  font-weight: 600;
  font-size: 18px;

  &.first {
    background-color: #ddd;
  }
}

.day-name {
  color: #888;
}

.min-max {
  display: flex;
  gap: 20px;

  span:last-child {
    color: #888;
  }
}
</style>