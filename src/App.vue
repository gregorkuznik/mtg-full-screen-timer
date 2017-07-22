<template>
  <div class="app-container">
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'app'
}
</script>

<style>
body {
  margin: 0px;
}
.app-container {
  display: flex;
  justify-content: center;
  align-items: center;
  font-family: "Times New Roman", Times, serif;
}
</style>
