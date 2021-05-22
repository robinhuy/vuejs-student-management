<template>
  <b-container>
    <h1 class="text-center">Student Management</h1>

    <div class="d-flex justify-content-end align-items-center mt-5">
      <label for="search">Search</label>

      <b-form-input
        id="search"
        class="ms-2"
        style="max-width: 350px"
        v-model="searchKeyword"
        type="search"
        placeholder="Type to Search"
        debounce="500"
      ></b-form-input>
    </div>

    <b-table
      id="table-users"
      class="mt-4"
      :striped="true"
      :hover="true"
      :bordered="true"
      :busy.sync="isBusy"
      :items="userProvider"
      :fields="fields"
      :current-page="currentPage"
      :per-page="perPage"
      :sort-by.sync="sortBy"
      :sort-desc.sync="sortDesc"
      :filter="searchKeyword"
    >
      <template #cell(index)="data">
        {{ data.index + 1 }}
      </template>

      <template #cell(avatar)="data">
        <img :src="data.value" />
      </template>
    </b-table>

    <b-pagination
      v-model="currentPage"
      :total-rows="totalRows"
      :per-page="perPage"
      aria-controls="table-users"
      align="center"
    ></b-pagination>
  </b-container>
</template>

<script>
import axios from "axios";

export default {
  name: "Home",
  data() {
    return {
      currentPage: 1,
      perPage: 3,
      totalRows: 0,
      sortBy: "firstName",
      sortDesc: false,
      searchKeyword: "",
      fields: [
        "index",
        "avatar",
        {
          key: "firstName",
          sortable: true,
        },
        {
          key: "birthday",
          sortable: true,
        },
        {
          key: "phone",
          sortable: true,
        },
      ],
      isBusy: false,
    };
  },
  methods: {
    async userProvider() {
      console.log("fetch", this.searchKeyword);
      let promise = axios.get(
        `http://localhost:3000/users?_page=${this.currentPage}&_limit=${
          this.perPage
        }&_sort=${this.sortBy}&_order=${this.sortDesc ? "desc" : "asc"}&q=${
          this.searchKeyword
        }`
      );

      return promise
        .then((res) => {
          this.totalRows = res.headers["x-total-count"];
          return res.data;
        })
        .catch((error) => {
          console.log(error);
          return [];
        });
    },
  },
};
</script>

<style>
tr > td {
  vertical-align: middle;
}
.sr-only {
  display: none;
}
</style>
