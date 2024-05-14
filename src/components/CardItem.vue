<template class="card">
  <article class="card">
    <div class="image__wrapper">
      <img class="hero-image" :src="hero.image" alt="" />
    </div>

    <div class="hero-info__wrapper">
      <span class="hero-name">{{ hero.name }}</span>
      <div class="hero-status__wrapper">
        <span class="hero-status">
          <span :class="hero.status.toLowerCase()" class="hero-status__icon"></span>
        </span>
        {{ hero.status }} - {{ hero.species }}
      </div>

      <span class="hero-secondary-text">Last known location:</span>
      <span class="hero-location">
        {{ hero.location.name }}
      </span>

      <span class="hero-secondary-text">First seen in:</span>
      <span class="hero-first-seen">
        {{ this.firstSeenEpisodeName }}
      </span>
    </div>
  </article>
</template>

<script>
import axios from "axios";
export default {
  setup() {
    return {};
  },
  props: { hero: Object },
  data() {
    return { firstSeenEpisodeName: null };
  },
  beforeMount() {
    axios.get(this.hero.episode[0]).then((response) => {
      this.firstSeenEpisodeName = response.data.name;
    });
  },
};
</script>

<style lang="scss" scoped>
.card {
  display: flex;
  color: red;
  min-width: 600px;
  min-height: 220px;
  background: rgb(60, 62, 68);
  border-radius: 10px;
}

.hero-name {
  font-size: 30px;
}

.hero-status {
  margin-right: 0.375rem;
  border-radius: 50%;
  width: 0.5rem;
  height: 0.5rem;
  .alive {
    background: rgb(85, 204, 68);
  }
  .unknown {
    background: rgb(165, 165, 165);
  }
  .dead {
    background: rgb(255, 53, 53);
  }
}

.hero-status__icon {
  width: 0.5rem;
  height: 0.5rem;
  display: block;
  border-radius: 10px;
}

.hero-image {
  width: 100%;
  height: 100%;
  margin: 0px;
  opacity: 1;
  transition: opacity 0.5s ease 0s;
  object-position: center center;
  object-fit: cover;
  border-radius: 10px;
}

.image__wrapper {
  flex: 2 1 0%;
  width: 100%;
}

.hero-info__wrapper {
  flex: 3 1 0%;
  position: relative;
  padding: 0.75rem;
  color: rgb(255, 255, 255);
  display: flex;
  flex-direction: column;
}

.hero-status__wrapper {
  display: flex;
  align-items: center;
}

.hero-secondary-text {
  color: rgb(158, 158, 158);
  margin-top: 15px;
}
</style>
