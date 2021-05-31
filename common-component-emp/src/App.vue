<template>
  <div class="main">
    <h3>Contents</h3>
    <img src="./logo.png" />
    <Contents />
  </div>
</template>

<script>
import Contents from './components/Contents';
// import Content from '@v2base/Content';
export default {
    components: {
        Contents
    }
};
</script>

<style scoped>
.main {
  text-align: center;
}

img {
  width: 200px;
}

h1 {
  font-family: Arial, Helvetica, sans-serif;
}
</style>
