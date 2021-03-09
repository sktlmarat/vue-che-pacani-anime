<template>
  <div id="app">
    <header>
      <h1>🥷 Che Pacani <strong>Anime?</strong></h1>
    </header>
    <form action="" class="search-box"  @submit.prevent="HandleSearch">
      <input
        type="search"
        class="search-field"
        placeholder="Search for an anime..."
        v-model="search_query"
        required
      />
    </form>
    <main>
      <div class="cards" v-if="animelist.length > 0">
        <Card
          v-for="anime in animelist"
          :key="anime.mal_id"
          :anime="anime"
        ></Card>
      </div>
      <div class="no-results" v-else>
        <h3>Sorry, we have no results...</h3>
      </div>
    </main>
  </div>
</template>

<script>
import { ref } from "vue";
import Card from "./components/Card";

export default {
  setup() {
    const search_query = ref("");
    const animelist = ref([]);
    const HandleSearch = async () => {
      animelist.value = await fetch(
        `https://api.jikan.moe/v3/search/anime?q=${search_query.value}`
      )
        .then((res) => res.json())
        .then((data) => data.results);
      search_query.value = "";
    };
    return {
      Card,
      search_query,
      animelist,
      HandleSearch,
    };
  },
};
</script>

<style scoped lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
strong {
  color: red;
}
.search-box {
  display: flex;
  justify-content: center;
  padding-left: 30px;
  padding-right: 30px;
  margin-bottom: 50px;
  .search-field {
    appearance: none;
    background: none;
    border: none;
    outline: none;
    background-color: #f3f3f3;
    box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.15);
    display: block;
    width: 100%;
    max-width: 600px;
    padding: 15px;
    border-radius: 8px;
    color: #313131;
    font-size: 20px;
    transition: 0.4s;
    &::placeholder {
      color: #aaa;
    }
    &:focus,
    &:valid {
      color: #fff;
      background-color: #313131;
      box-shadow: 0px 0px 0px rgba(0, 0, 0, 0.15);
    }
  }
}
main {
  max-width: 1200px;
  margin: 0 auto;
  padding-left: 30px;
  padding-right: 30px;
  .cards {
    display: flex;
    flex-wrap: wrap;
    margin: 0 -8px;
  }
}
</style>
