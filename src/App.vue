<template>
  <div id="nav">
    <router-link to="/">Home</router-link> |
    <router-link to="/about">About</router-link>
  </div>
  <router-view/>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}

.select2-container--default .select2-selection--single .select2-selection__arrow {
  top: 6px !important;
}

.select2-container .select2-selection--single {
  height: calc(1.5em + .75rem + 2px) !important;
}

.select2-container--default .select2-selection--single .select2-selection__rendered {
  line-height: calc(1.5em + .75rem + 2px) !important;
}

.select2-container--default.select2-container--open.select2-container--below .select2-selection--single:focus {
  outline: none !important;
}

.error-text {
  font-size: 12px;
  color: #e03c3c;
}
</style>
