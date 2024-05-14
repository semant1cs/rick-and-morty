<template>
  <div class="slider__btns">
    <button @click="decrementPageIndex"><</button>
    <button @click="incrementPageIndex">></button>
    <span class="pages-slider">{{ currentPageIndex }}/{{ this.pagesCount }}</span>
  </div>

  <select v-model="selected">
    <option v-for="option in options">{{ option.text }}</option>
  </select>

  <input v-model="inputNameFilter" />
  <button @click="fetchCharactersInfo">Поиск</button>

  <CardList v-if="isDataExists" :heroes="this.heroes" />
  <div v-else class="error-message">Похоже, что ничего не нашли</div>
</template>

<script>
import axios from "axios";
import { ref } from "vue";
import CardList from "../components/CardList.vue";

export default {
  setup() {
    const currentPageIndex = ref(1);
    const inputNameFilter = ref("");

    return { currentPageIndex, inputNameFilter };
  },
  data() {
    return {
      pagesCount: null,
      heroesCount: null,
      heroes: null,
      selected: "",
      isDataExists: false,
      options: [
        { text: "Alive", value: "alive" },
        { text: "Dead", value: "dead" },
        { text: "Unknown", value: "unknown" },
      ],
    };
  },
  methods: {
    incrementPageIndex() {
      if (this.currentPageIndex < this.pagesCount) {
        this.currentPageIndex++;
        this.fetchCharactersInfo();
      }
    },

    decrementPageIndex() {
      if (this.currentPageIndex > 1) {
        this.currentPageIndex--;
        this.fetchCharactersInfo();
      }
    },
    fetchCharactersInfo() {
      axios
        .get(`https://rickandmortyapi.com/api/character?page=${this.currentPageIndex}&status=${this.selected}&name=${this.inputNameFilter}`)
        .then((response) => {
          this.isDataExists = true;
          const data = response.data;
          this.pagesCount = data.info.pages;
          this.heroesCount = data.info.count;
          this.heroes = data.results;
        })
        .catch(() => {
          this.isDataExists = false;
        });
    },
  },
  beforeMount() {
    this.fetchCharactersInfo();
  },
  components: {
    CardList: CardList,
  },
};
</script>

<style lang="scss" scoped>
.slider__btns {
  display: flex;
  gap: 10px;
  color: white;

  button {
    background-color: rgb(60, 62, 68);
    color: white;
  }
  .pages-slider {
    background-color: rgb(60, 62, 68);
    border-radius: 10px;
    padding: 11px;
    margin-left: 20px;
  }
}

.error-message {
  color: white;
}
</style>
