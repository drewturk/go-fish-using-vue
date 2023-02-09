<template>
  <div class="home">
    <h2 class="header">
      We love to eat healthy, and we love to eat fresh fish, so we’d like to
      build in an area where the fish are low in calories and fat.
    </h2>
    <div class="container">
      <card-component
        v-for="(region, index) in regionData"
        :key="index"
        :title="region.regionName"
        v-on:click.native="goToRegion(region)"
      >
        <div>Average per Fish:</div>
        <div style="font-weight: bold">
          {{ `Calories: ${region.avgCalories.toFixed(0)}` }}
        </div>
        <div style="font-weight: bold">
          {{ `Fat Content: ${region.avgFat.toFixed(2)} g` }}
        </div>
      </card-component>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { groupBy, sumBy, map } from "lodash";
// @ is an alias to /src
import CardComponent from "@/components/CardComponent.vue";

export default {
  name: "HomeView",
  components: {
    CardComponent,
  },
  async mounted() {
    this.getRegionData();
  },
  data() {
    return {
      regionData: [],
    };
  },
  methods: {
    goToRegion(region) {
      console.log(`goTo ${region}`);
      this.$router.push({
        name: "region",
        params: { name: region.regionName, data: region },
      });
    },
    getRegionData() {
      const apiURL = axios.create({
        baseURL: "http://localhost:5001",
      });
      apiURL
        .get("gofish?apikey=abrradiology")
        .then((response) => {
          // console.log(response.data);
          this.transformRegionData(
            groupBy(response.data, "NOAAFisheriesRegion")
          );
        })
        .catch((e) => console.log(e));
    },
    transformRegionData(data) {
      this.regionData = map(data, function (region, regionName) {
        const avgCalories = sumBy(region, function (o) {
          // TODO: check for NaN
          return parseInt(o.Calories) / region.length;
        });
        const avgFat = sumBy(region, function (o) {
          // TODO: check for NaN
          return parseInt(o.FatTotal) / region.length;
        });
        // list of fish
        const fish = map(region, function (o) {
          return {
            SpeciesName: o.SpeciesName,
            ImageGallery: o.ImageGallery,
            Calories: o.Calories,
            FatTotal: o.FatTotal,
            PhysicalDescription: o.PhysicalDescription,
          };
        });
        return { regionName, avgCalories, avgFat, fish };
      });
    },
  },
};
</script>

<style lang="scss">
.header {
  margin-bottom: 25px;
}
.container {
  padding: 4px 16px;
}
</style>
