<template>
  <header class="bg-primary d-flex justify-content-center align-items-center">
    <h5 class="text-white"><b>Frontend Talentu</b></h5>
  </header>
  <router-view/>
</template>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

header {
  height: 60px;
  box-shadow: 0px 3px 15px 5px rgba(#0d6efd, 0.2);
}

h5 {
  margin-bottom: 0px;
}
</style>