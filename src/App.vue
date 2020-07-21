<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">FIO Price Voting Tool</router-link>
      <b-col cols="1">
        <b-img center src="@/assets//fio-logo.png" width="200"></b-img>
      </b-col>
    </div>
    <router-view />
  </div>
</template>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
