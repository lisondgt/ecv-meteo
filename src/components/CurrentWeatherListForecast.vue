<template>
  <div class="container list-forecast">
    <ion-card>
      <ion-card-header>
        <ion-card-subtitle>Prévisions sur 24H</ion-card-subtitle>
      </ion-card-header>
      <ion-card-content class="custom-weather-card">
        <ion-list class="list-weather">
          <ion-item color="transparent" v-for="(forecast, index) in forecasts" :key="index">
            <ion-avatar>
              <img
                :src="`http://openweathermap.org/img/wn/${forecast.weather.icon}.png`"
              />
            </ion-avatar>
            <ion-label>
              <h2>{{ forecast.hour }}</h2>
              <h3>{{ forecast.temperature }}</h3>
              <p>{{ forecast.weather.description }}</p>
            </ion-label>
          </ion-item>
        </ion-list>
      </ion-card-content>
    </ion-card>
  </div>
</template>

<script lang="ts">
export default {
  name: "CurrentWeatherListForecast",
  props: {
    forecasts: Array,
  },
};
</script>