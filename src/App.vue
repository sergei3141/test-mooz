<template>
  <div id="app">
    <MainHeader @search="onSearch" />
    <FilmList :searchQuery="searchQuery" />
  </div>
</template>

<script>
import MainHeader from './components/MainHeader.vue';
import FilmList from './components/FilmList.vue';

export default {
  components: {
    MainHeader,
    FilmList,
  },
  data() {
    return {
      searchQuery: '',
    };
  },
  methods: {
    onSearch(query) {
      this.searchQuery = query;
    },
  },
};
</script>