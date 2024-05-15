<template>
  <div class="card-list">
    <div v-for="hero in heroes">
      <CardItem :key="hero.id" :hero="hero" />
    </div>
  </div>
</template>

<script lang="ts">
import CardItem from "./CardItem.vue";

export default {
  setup() {
    return {};
  },
  props: { heroes: { type: Object, required: true } },
  components: {
    CardItem: CardItem,
  },
};
</script>

<style lang="scss" scoped>
.card-list {
  display: grid;
  grid-template-columns: repeat(auto-fill, 600px);
  grid-template-rows: 1fr;
  grid-column-gap: 30px;
  grid-row-gap: 100px;
  justify-items: stretch;
  align-items: stretch;
  margin-top: 70px;
  margin-left: 20px;
  row-gap: 30px;
}
</style>
