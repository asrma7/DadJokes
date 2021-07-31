<template>
  <div>
    <div class="pagination">
      <div class="previous" @click="onPageBack">Previous</div>
      <ul class="paginate-ul">
        <li :class="currentPage == 1 ? 'active' : ''" @click="onPageClick(1)">
          1
        </li>
        <li class="spacer" v-if="currentPage > 3">...</li>
        <li
          v-for="page in pages"
          :key="page"
          :class="currentPage == page ? 'active' : ''"
          @click="onPageClick(page)"
        >
          {{ page }}
        </li>
        <li class="spacer" v-if="currentPage < totalPages - 2">...</li>
        <li
          :class="currentPage == totalPages ? 'active' : ''"
          @click="onPageClick(totalPages)"
        >
          {{ totalPages }}
        </li>
      </ul>
      <div class="next" @click="onPageForward">Next Page</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Pagination",
  props: ["currentPage", "totalPages"],
  computed: {
    pages() {
      function range(start, end) {
        return Array.from(Array(end - start + 1), (_, i) => i + start);
      }

      const max = 3;

      if (this.totalPages < 5) {
        return range(2, this.totalPages - 1);
      }

      let start = this.currentPage - 1;
      let end = this.currentPage <= 2 ? 4 : this.currentPage + 1;

      // If we're close to the end
      if (this.currentPage >= this.totalPages - 1) {
        start = this.totalPages - max;
        end = this.totalPages - 1;
      }

      return range(Math.max(2, start), Math.max(end, max - 1));
    },
  },
  methods: {
    onPageClick(page) {
      this.$emit("change-page", page);
    },
    onPageBack() {
      if (this.currentPage - 1 >= 1) {
        this.onPageClick(this.currentPage - 1);
      }
    },
    onPageForward() {
      if (this.currentPage + 1 <= 33) {
        this.onPageClick(this.currentPage + 1);
      }
    },
  },
};
</script>

<style>
.pagination {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.paginate-ul li {
  display: inline-block;
}
.previous,
.next,
.paginate-ul li {
  color: #00d1b2;
  border: 1px solid #b5b5b5;
  width: 35px;
  height: 35px;
  text-align: center;
  line-height: 35px;
  border-radius: 3px;
  margin: 0 0.2rem;
  cursor: pointer;
}

.paginate-ul li.spacer {
  border: none;
  color: #b5b5b5;
  width: auto;
}

.previous,
.next {
  width: 100px;
}

.paginate-ul li.active {
  background-color: #00d1b2;
  color: #fff;
  border: 1px solid #00d1b2;
}
</style>