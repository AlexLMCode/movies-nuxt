<template>
  <div class="container search">
    <input
      v-model.lazy="searchInput"
      type="text"
      placeholder="Search"
      @keyup.enter="$emit('searchValue', searchInput)"
    />
    <button
      v-show="searchInput !== ''"
      class="button"
      @click="$emit('clearSearch', searchInput)"
    >
      Clear Search
    </button>
  </div>
</template>

<script>
export default {
  name: 'Search',
  props: {
    searchVal: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      searchInput: '',
    }
  },
  watch: {
    searchVal(newVal, oldVal) {
      this.searchInput = newVal
    },
  },
}
</script>

<style lang="scss" scoped>
.search {
  display: flex;
  padding: 32px 16px;
  input {
    max-width: 1000px;
    width: 100%;
    padding: 12px 6px;
    font-size: 14px;
    border: none;
    &:focus {
      outline: none;
    }
  }
  .button {
    border-top-left-radius: 0;
    border-bottom-left-radius: 0;
  }
}
</style>