<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Rechercher une ville</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Rechercher une ville</ion-title>
        </ion-toolbar>
      </ion-header>
      <div class="container-search">
        <ion-input v-model="city" clear-input value=""></ion-input>
        <ion-button @click="searchWeather()" expand="block"
          >Rechercher</ion-button
        >
      </div>

      <div class="container-button-fav">
        <ion-button @click="addToFavorites()" expand="block"
          >Ajouter en favoris</ion-button
        >
      </div>

      <current-weather
        v-if="coordinates"
        :latitude="coordinates.latitude"
        :longitude="coordinates.longitude"
      />
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import {
  IonPage,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonInput,
  IonContent,
} from "@ionic/vue";
import coordinatesService from "../services/coordinates.service";
import CurrentWeather from "../components/CurrentWeather.vue";
import { Coordinates } from "@/interfaces/coordinates.interface";
import { mapActions, mapState } from "pinia";
import { cityStore } from "@/store/city.store";

export default {
  components: {
    CurrentWeather,
    IonHeader,
    IonInput,
    IonToolbar,
    IonTitle,
    IonContent,
    IonPage,
  },
  data() {
    return {
      city: "",
      coordinates: null,
    };
  },
  async mounted() {
    if (this.$route.query.city) {
      this.city = this.$route.query.city;
      await this.searchWeather();
    }
  },
  computed: {
    ...mapState(cityStore, ["favoritesCity"]),
  },
  methods: {
    ...mapActions(cityStore, ["addNewCity"]),

    async searchWeather(): Promise<void> {
      this.coordinates = null;
      const coordinates = await coordinatesService.getCoordinatesByCityName(
        this.city
      );
      this.coordinates = coordinates as Coordinates;
    },

    addToFavorites(): void {
      return this.addNewCity(this.city);
    },
    checkIfCityAlreadyInFavorites(): boolean {
      const isExistCity: Array<string> = this.favoritesCity.filter(
        (city: string) => city === this.city
      ).length;
      console.log(isExistCity);
      return isExistCity.length > 0 ? true : false;
    },
  },
};
</script>

<style scoped>
.container-search,
.container-button-fav {
  margin: 24px 16px;
}
.container-search .sc-ion-input-ios-h {
  border: 1px solid #666666;
  border-radius: 8px;
  padding: 5px 10px !important;
}
.container-search .button {
  margin: 5px 0 0 0;
}
</style>