<template>
  <b-container class="login-form">
    <h1>Login</h1>

    <b-form @submit="onSubmit">
      <b-form-group label="Username" label-for="username">
        <b-form-input
          id="username"
          v-model="form.username"
          type="text"
          placeholder="Enter username"
          required
        ></b-form-input>
      </b-form-group>

      <b-form-group label="Password" label-for="password">
        <b-form-input
          id="password"
          v-model="form.password"
          type="password"
          placeholder="Enter password"
          required
        ></b-form-input>
      </b-form-group>

      <b-button type="submit" variant="primary" class="mt-3">Login</b-button>
    </b-form>
  </b-container>
</template>

<script>
export default {
  name: "Login",
  data() {
    return {
      form: {
        username: "",
        password: "",
      },
    };
  },
  methods: {
    onSubmit(event) {
      event.preventDefault();
      this.$router.push("/");
    },
  },
};
</script>

<style scoped>
.login-form {
  max-width: 500px;
}
.form-group {
  margin-top: 15px;
}
</style>
