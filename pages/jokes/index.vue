<template>
  <div>
    <div class="search-bar">
      <b>{{ totalJokes }}</b
      ><span>jokes found</span>
      <SearchJokes v-on:search-text="searchText" />
    </div>
    <Pagination
      :currentPage="currentPage"
      :totalPages="totalPages"
      v-on:change-page="changePage"
      v-if="totalPages > 1"
    />
    <Joke
      v-for="joke in jokes"
      :key="joke.id"
      :id="joke.id"
      :joke="joke.joke"
    />
    <Pagination
      :currentPage="currentPage"
      :totalPages="totalPages"
      v-on:change-page="changePage"
      v-if="totalPages > 1"
    />
  </div>
</template>

<script>
import axios from "axios";
import Joke from "../../components/Joke.vue";
import Pagination from "../../components/Pagination.vue";
import SearchJokes from "../../components/SearchJokes.vue";
export default {
  components: {
    Joke,
    Pagination,
    SearchJokes,
  },
  data() {
    return {
      totalJokes: 0,
      currentPage: 1,
      totalPages: 0,
      term: "",
      jokes: [],
    };
  },
  async created() {
    const config = {
      headers: {
        Accept: "Application/json",
      },
    };

    try {
      const res = await axios.get(`https://icanhazdadjoke.com/search`, config);
      this.jokes = res.data.results;
      this.totalJokes = res.data.total_jokes;
      this.currentPage = res.data.current_page;
      this.totalPages = res.data.total_pages;
      this.term = res.data.search_term;
    } catch (err) {
      console.log(err);
    }
  },
  methods: {
    async changePage(page) {
      const config = {
        headers: {
          Accept: "Application/json",
        },
      };

      try {
        const res = await axios.get(
          `https://icanhazdadjoke.com/search?term=${this.term}&page=${page}`,
          config
        );
        this.jokes = res.data.results;
        this.totalJokes = res.data.total_jokes;
        this.currentPage = res.data.current_page;
        this.totalPages = res.data.total_pages;
        this.term = res.data.search_term;
      } catch (err) {
        console.log(err);
      }
    },
    async searchText(text) {
      const config = {
        headers: {
          Accept: "Application/json",
        },
      };

      try {
        const res = await axios.get(
          `https://icanhazdadjoke.com/search?term=${text}`,
          config
        );
        this.jokes = res.data.results;
        this.totalJokes = res.data.total_jokes;
        this.currentPage = res.data.current_page;
        this.totalPages = res.data.total_pages;
        this.term = res.data.search_term;
      } catch (err) {
        console.log(err);
      }
    },
  },
  head() {
    return {
      title: "Dad Jokes",
      meta: [
        {
          hid: "description",
          name: "description",
          content: "Best place for corny dad jokes",
        },
      ],
    };
  },
};
</script>

<style>
.search-bar {
  display: flex;
  align-items: center;
  margin: 10px 0;
}
.search-bar span {
  margin: 0 0.3rem;
}
</style>