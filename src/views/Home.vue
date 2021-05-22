<template>
  <b-container>
    <h1 class="text-center">Student Management</h1>

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
// import TableUserList from "@/components/TableUserList.vue";
import axios from "axios";

export default {
  name: "Home",
  components: {},
  data() {
    return {
      currentPage: 1,
      perPage: 3,
      totalRows: 0,
      sortBy: "firstName",
      sortDesc: false,
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
      let promise = axios.get(
        `http://localhost:3000/users?_page=${this.currentPage}&_limit=${
          this.perPage
        }&_sort=${this.sortBy}&_order=${this.sortDesc ? "desc" : "asc"}`
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
.sr-only {
  display: none;
}
</style>
