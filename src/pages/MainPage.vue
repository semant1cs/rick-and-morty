<template>
  <div class="search-hero-tools">
    <div class="slider__btns">
      <button @click="decrementPageIndex"><</button>
      <button @click="incrementPageIndex">></button>
      <span class="pages-slider" v-show="pagesCount > 0">{{ currentPageIndex }}/{{ pagesCount }}</span>
    </div>

    <select v-model="selected" class="status-hero__select">
      <option v-for="option in options">{{ option.value }}</option>
    </select>

    <input v-model="inputNameFilter" class="name-hero__input" />
    <button @click="fetchCharactersInfo" class="search-heroes__btn">Поиск</button>
  </div>

  <CardList v-if="isDataExists" :heroes="heroes" :handleScrollPagination="handleScrollPagination" />
  <div v-else class="error-message">Похоже, что ничего не нашли</div>
</template>

<script lang="ts">
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
      pagesCount: 0,
      heroes: [],
      selected: "",
      isDataExists: false,
      options: [{ value: null }, { value: "alive" }, { value: "dead" }, { value: "unknown" }],
    };
  },
  methods: {
    incrementPageIndex() {
      if (this.currentPageIndex < this.pagesCount) {
        this.currentPageIndex++;
        this.fetchCharactersInfo();
        this.scrollPageUp();
      }
    },

    decrementPageIndex() {
      if (this.currentPageIndex > 1) {
        this.currentPageIndex--;
        this.fetchCharactersInfo();
        this.scrollPageUp();
      }
    },

    scrollPageUp() {
      const cardList = document.querySelector(".card-list");
      if (cardList) cardList.scrollTop = 0;
    },

    fetchCharactersInfo() {
      axios
        .get(`https://rickandmortyapi.com/api/character?page=${this.currentPageIndex}&status=${this.selected}&name=${this.inputNameFilter}`)
        .then((response) => {
          this.isDataExists = true;
          const data = response.data;
          this.pagesCount = data.info.pages;
          this.heroes = data.results;
        })
        .catch(() => {
          this.isDataExists = false;
          this.pagesCount = 0;
          this.heroes = [];
        });
    },
    handleScrollPagination() {
      if (this.currentPageIndex < this.pagesCount) {
        setTimeout(() => {
          this.currentPageIndex++;
          axios
            .get(`https://rickandmortyapi.com/api/character?page=${this.currentPageIndex}&status=${this.selected}&name=${this.inputNameFilter}`)
            .then((response) => {
              const data = response.data;
              this.pagesCount = data.info.pages;
              this.heroes.concat(data.results);
              this.heroes = this.heroes.concat(data.results);
            });
        }, 1000);
      }
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
.search-hero-tools {
  margin-left: 20px;
}

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

.status-hero__select {
  margin-top: 10px;
}

.status-hero__select,
.name-hero__input,
.search-heroes__btn {
  background: rgb(60, 62, 68);
  width: 205px;
  height: 40px;
  border-radius: 10px;
  border: 1px solid transparent;
  padding: 0;
  padding-left: 10px;
}

.name-hero__input {
  margin-left: 20px;
  transition: 0.2s ease-in-out;
  &:focus {
    outline: none;
  }
  &:hover {
    border: 1px solid white;
    transition: 0.2s ease-in-out;
  }
}

.search-heroes__btn {
  margin-left: 20px;
  color: white;
  padding: 0;

  &:hover {
    border: 1px solid white;
    transition: 0.2s ease-in-out;
  }
}
</style>
