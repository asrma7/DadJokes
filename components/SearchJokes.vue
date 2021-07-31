<template>
  <form @submit.prevent="onSubmit" class="search-form">
    <input type="text" v-model="text" placeholder="Search Jokes..." />
    <input type="submit" value="Search" />
  </form>
</template>

<script>
export default {
  name: "SearchJokes",
  data() {
    return {
      text: "",
    };
  },
  methods: {
    onSubmit() {
      this.$emit("search-text", this.text);
      this.text = "";
    },
  },
};
</script>

<style>
.search-form {
  display: flex;
}

.search-form input {
  height: 35px;
  padding: 0 10px;
  border: 1px solid #ccc;
  outline: none;
  border-radius: 0;
}

.search-form input[type="text"] {
  border-right: none;
  border-top-left-radius: 5px;
  border-bottom-left-radius: 5px;
}

.search-form input[type="submit"] {
  background-color: #5555ff;
  border: 0;
  color: #fff;
  border-top-right-radius: 5px;
  border-bottom-right-radius: 5px;
}
</style>