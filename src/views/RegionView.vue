<template>
  <div>
    <h2>{{ data.regionName }}</h2>
    <h3>Average Calories and Fat for this region:</h3>
    <div>{{ `${data.avgCalories.toFixed(0)} cal` }}</div>
    <div>{{ `${data.avgFat.toFixed(2)} g` }}</div>
    <div class="container">
      <card-component
        v-for="(fish, index) in data.fish"
        :key="index"
        :title="fish.SpeciesName"
      >
        <img
          :src="
            fish.ImageGallery?.src ||
            (Array.isArray(fish.ImageGallery) && fish.ImageGallery[0].src)
          "
          :alt="
            fish.ImageGallery?.alt ||
            (Array.isArray(fish.ImageGallery) && fish.ImageGallery[0].alt)
          "
          style="width: 100%"
        />
        <div class="nutrition">
          Nutrition: <span>{{ `${fish.Calories} cal` }}</span>
          <span>{{ `${fish.FatTotal}` }}</span>
        </div>
        <div class="description" v-html="fish.PhysicalDescription"></div>
      </card-component>
    </div>
  </div>
</template>

<script>
import CardComponent from "@/components/CardComponent.vue";
export default {
  components: { CardComponent },
  props: {
    data: Object,
  },
};
</script>

<style lang="scss">
.nutrition {
  font-weight: bold;
}
</style>
