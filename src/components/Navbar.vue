<template>
  <nav class="navbar navbar-default">
    <div class="container">
      <div class="navbar-header">
        <router-link class="navbar-brand" to="/">FB Albums</router-link>
      </div>
      <ul class="nav navbar-nav navbar-right">
        <!-- <fb:login-button ></fb:login-button> -->
        <button v-if="$parent.authorized" @click="$parent.logout()" class="btn btn-primary" id="logout">Logout</button>
        <button v-if="!$parent.authorized" @click="$parent.login()" class="btn btn-primary" id="login">Login</button>
      </ul>
    </div>
  </nav>
</template>

<script>
export default {

}
</script>

<style scoped>
  #login, #logout{margin-top:7px;}
</style>