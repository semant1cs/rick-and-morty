<template>
  <ul class="card-list">
    <li v-for="hero in heroes">
      <CardItem :key="hero.id" :hero="hero" />
    </li>
  </ul>
  <transition name="fade">
    <div class="loading" v-show="loading"><span class="fa fa-spinner fa-spin"></span> Loading</div>
  </transition>
</template>

<script lang="ts">
import CardItem from "./CardItem.vue";

export default {
  setup() {
    return {};
  },
  props: { heroes: { type: Object, required: true }, handleScrollPagination: { type: Function, required: true } },
  components: {
    CardItem: CardItem,
  },
  data() {
    return { loading: false };
  },

  mounted() {
    const listElm = document.querySelector(".card-list");

    listElm?.addEventListener("scroll", () => {
      if (listElm.scrollTop + listElm.clientHeight >= listElm.scrollHeight) {
        this.loading = true;
        this.handleScrollPagination();
        setTimeout(() => {
          this.loading = false;
        }, 600);
      }
    });
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
  list-style-type: none;
  padding: 0;
  overflow: auto;
  height: 70vh;
}

.loading {
  text-align: center;
  color: #fff;
  z-index: 9;
  background-color: rgb(60, 62, 68);
  padding: 8px 18px;
  border-radius: 5px;
  max-width: 70px;
  max-height: 30px;
  left: calc(50% - 45px);
  top: calc(80% - 18px);
  margin-left: 20px;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
