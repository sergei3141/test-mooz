<template>
  <nav aria-label="Page navigation example">
    <ul class="pagination">
      <li class="page-item" :key="page" :class="{ disabled: currentPage === 1 }">
        <button class="page-link-corner" @click="goToPage(1)">В начало</button>
      </li>
      <li class="page-item" :class="{ disabled: currentPage === 1 }">
        <button class="page-link" @click="previousPage">
          <i class="fas fa-chevron-left"><</i>
        </button>
      </li>
      <li class="page-item" v-for="page in totalPages" :key="page" :class="{ active: currentPage === page }">
        <button class="page-link" @click="goToPage(page)">{{ page }}</button>
      </li>
      <li class="page-item" :class="{ disabled: currentPage === totalPages }">
        <button class="page-link" @click="nextPage">
          <i class="fas fa-chevron-right">></i>
        </button>
      </li>
      <li class="page-item" :key="page" :class="{ disabled: currentPage === totalPages }">
        <button class="page-link-corner" @click="goToPage(totalPages)">В конец</button>
      </li>
    </ul>
  </nav>
</template>

<script>
export default {
  props: {
    totalResults: {
      type: Number,
      required: true
    }
  },
  data() {
    return {
      currentPage: 1,
      perPage: 10
    }
  },
  computed: {
    totalPages() {
      return Math.ceil(this.totalResults / this.perPage)
    }
  },
  methods: {
    previousPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
        this.$emit('page-changed', "", this.currentPage);
      }
    },
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
        this.$emit('page-changed', "", this.currentPage);
      }
    },
    goToPage(page) {
      this.currentPage = page;
      this.$emit('page-changed', "", this.currentPage);
    }
  }
}
</script>

<style scoped lang="scss">
  .pagination {
    display: flex;
    justify-content: center;
    align-items: center;
    list-style: none;
    padding: 0;
    margin: 0;
  }

  .page-item {
    margin: 40px 5px;
  }

  .page-link, .page-link-corner {
    padding: 0.5rem 0.75rem;
    border: 1px solid #ddd;
    border-radius: 0.25rem;
    background-color: #fff;
    color: #333;
    text-decoration: none;
    font-size: 14px;
    cursor: pointer;
  }

  .page-item.active .page-link {
    background-color: #007bff;
    border-color: #007bff;
    color: #fff;
  }

  .page-item.disabled .page-link {
    cursor: default;
    color: #999;
  }

  @media screen and (max-width: 530px) {
    .page-link-corner {
      display: none;
    }
  }
  
</style>