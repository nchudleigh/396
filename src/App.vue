<template>
  <div id="app">
    <router-view></router-view>
  </div>
</template>

<script>
export default {
    name: 'app'
}
</script>

<style>
#app {

}

body {
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    font-family: Helvetica Neue,Helvetica,PingFang SC,Hiragino Sans GB,Microsoft YaHei,SimSun,sans-serif;
}

.m1 {
    margin: 1rem;
}

.mp25 {
    margin: .25rem;
}

.bold {
    font-weight: 600;
}

.green{
    color: green;
}
.red{
    color: red  ;
}
.ib {
    display: inline-block;
}
</style>
