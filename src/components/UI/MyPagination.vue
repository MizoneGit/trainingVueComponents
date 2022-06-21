<template>
  <div class="page__wrapper">
    <div @click="changePage(pageNumber)" v-for="pageNumber in totalPages" :key="pageNumber" class="page" :class="{'page--active': pageNumber === currentPage }">{{ pageNumber }}</div>
  </div>
</template>

<script>
export default {
  name: "my-pagination",
  props: {
    currentPage: {
      type: Number,
      default: 1
    },
    totalPages: {
      type: Number,
      default: 0
    }
  },
  methods: {
    changePage(pageNumber) {
      this.$emit('update:currentPage', pageNumber);
    }
  }
}
</script>

<style scoped>
  .page__wrapper {
    display: flex;
    margin-top: 20px;
  }
  .page {
    border: 1px solid #3A68B8;
    padding: 5px 10px;
  }
  .page--active {
    border: 1px solid #3A68B8;
    background: #3A68B8;
    color: #FFFFFF;
  }
</style>