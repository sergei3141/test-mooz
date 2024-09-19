<template>
  <header>
    <div class="header__logo">Online Cinema</div>
    <div class="header__search">
      <input type="text" placeholder="Поиск фильмов" v-model="searchQuery" @keyup.enter="onSearch">
    </div>
    <div class="header__user">
      <img src="https://placehold.co/50x50" alt="Фото пользователя">
      <div class="header__username">Sergey_3141 ▾</div>
    </div>
  </header>
</template>


<script>

export default {
  data() {
    return {
      searchQuery: '',
      movies: [],
    };
  },
  methods: {
    onSearch() {
      this.$emit('search', this.searchQuery);
    },
  },
};
</script>
  

<style scoped lang="scss">
  header {
  display: flex;
  align-items: center;
  padding: 20px;
  background-color: $secondary-color;
}

.header__logo {
  font-size: 36px;
  flex-grow: 5;
  margin-right: 30px;
}

.header__search {
  flex-grow: 65;
}

.header__search input {
  padding: 10px;
  margin-top: 5px;
  border: 1px solid #ccc;
  border-radius: 4px;
  width: 100%;
  box-sizing: border-box;
}

.header__user {
  display: flex;
  align-items: center;
  justify-content: right;
  flex-grow: 25;
  margin-left: 30px;
  font-size: 20px;
}

.header__user img {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  margin-right: 10px;
}

@media screen and (max-width: 768px) {
  header {
    display: block;
  }

  .header__logo {
    justify-content: center;
    text-align: center;
  }

  .header__user {
    display: none;
  }
}
  </style>
  