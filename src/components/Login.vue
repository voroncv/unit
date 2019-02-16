<template>
  <div class="login">
    <div class="form">
      <p>Sing in</p>
      <input type="text" placeholder="E-mail" v-model="login">
      <input type="password" placeholder="password" v-model="password">
      <button @click="singInUser()">Sing in</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Login',
  data() {
    return {
      login: '',
      password: '',
    }
  },
  methods: {
    singInUser() {
      const {
        login,
        password
      } = this

      if (!!login && !!password) {
        this.$router.push('/editor')
      }
    }
  }
};
</script>

<style scoped>
.form {
  max-width: 400px;
  margin: 0 auto;
  margin-top: 50px;
}
.form p {
  font-size: 16px;
  font-family: Tahoma;
  font-weight: 500;
  margin: 0;
}
.form input {
  margin: 10px 0;
  height: 30px;
  outline: none;
  font-size: 1rem;
  display: block;
  width: 100%;
  padding: .275rem .75rem;
}
.form button {
  background: #00a2e3;
  border-color: #00a2e3;
  color: #FFFFFF;
  font-size: 14px;
  margin: 10px 0;
  border-width: 2px;
  display: block;
  padding: .5rem 1rem;
  width: 50%;
}
.form button:hover {
  cursor: pointer;
}
</style>
