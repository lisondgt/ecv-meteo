<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Liste des favoris</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Liste des favoris</ion-title>
        </ion-toolbar>
      </ion-header>
      
      <ion-card v-for="(city, index) in favoritesCity" :key="index">
        <ion-card-title @click="showWeatherLocation(city)">
          {{ city }}
          </ion-card-title>
          <ion-card-content>
            <a @click="getCurrentCity(index)" class="link-delete"><ion-icon :icon="trash"></ion-icon></a>
          </ion-card-content>
      </ion-card>

    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import {
  IonPage,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonContent,
} from "@ionic/vue";
import { trash } from "ionicons/icons";
import { mapActions, mapState } from "pinia";
import { cityStore } from "../store/city.store";

export default {
  components: { IonHeader, IonToolbar, IonTitle, IonContent, IonPage },
  computed: {
    ...mapState(cityStore, ["favoritesCity"]),
  },
   data() {
    return {
      trash,
    };
  },
  methods: {
    ...mapActions(cityStore, ["deleteCity"]),
    showWeatherLocation(city: string): void {
      window.location.href = `/tabs/searchWeather?city=${city}`;
    },
    getCurrentCity(index: number): void {
      return this.deleteCity(index);
    },
  },
};
</script>

<style scoped>

ion-card {
  padding: 20px 30px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

ion-card-content {
  padding: 0;
}

ion-icon {
  width: 20px;
  height: 20px;
}

.link-delete {
  color: grey;
}

</style>