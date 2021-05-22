<template>
  <div class="home">
    <b-container>
      <h1>Student Management</h1>

      <b-table :busy.sync="isBusy" :items="userProvider" :fields="fields">
        <template #cell(index)="data">
          {{ data.index + 1 }}
        </template>

        <template #cell(avatar)="data">
          <img :src="data.value" />
        </template>
      </b-table>
    </b-container>
  </div>
</template>

<script>
// @ is an alias to /src
// import TableUserList from "@/components/TableUserList.vue";
import axios from "axios";

export default {
  name: "Home",
  components: {},
  data() {
    return {
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
    userProvider() {
      let promise = axios.get("http://localhost:3000/users");

      return promise
        .then((res) => {
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
