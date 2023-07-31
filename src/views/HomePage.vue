<template>
  <div class="ion-page">
    <ion-header>
      <ion-toolbar>
        <ion-title>ZipInfo</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content class="ion-padding">
      <!-- get-zip is the name used in ZipSearch.vue -->
      <zip-search v-on:get-zip="getZipInfo" />
      <zip-info v-bind:info="info" />
      <clear-info v-bind:info="info" v-on:clear-info="clearInfo" />
    </ion-content>
  </div>
</template>

<script setup>
import { alertController, IonContent, IonHeader, IonPage, IonTitle, IonToolbar } from '@ionic/vue';
import ZipSearch from '../components/ZipSearch.vue';
import ZipInfo from '../components/ZipInfo.vue';
import ClearInfo from '../components/ClearInfo.vue';
</script>

<script>
export default {
  data() {
    return {
      info: null
    };
  },
  methods: {
    // zip is passed from $emit("get-zip", this.zip)
    async getZipInfo(zip) {
      const res = await fetch(`https://api.zippopotam.us/us/${zip}`);
      if(res.status == 404) {
        this.showAlert();
      }
      // info is the one in data(), it will store the data obtained from res
      this.info = await res.json();
    },
    async showAlert() {
      return await alertController
        .create({
          header: "Enter Zipcode",
          message: "Please enter a valid US zipcode",
          buttons: ["OK"]
        })
        .then(a => a.present());
    },
    clearInfo() {
      this.info = null;
    }
  }
}
</script>


<style scoped>
#container {
  text-align: center;
  
  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
}

#container strong {
  font-size: 20px;
  line-height: 26px;
}

#container p {
  font-size: 16px;
  line-height: 22px;
  
  color: #8c8c8c;
  
  margin: 0;
}

#container a {
  text-decoration: none;
}
</style>
