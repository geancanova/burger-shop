<template>
  <div id="main-banner">
    <h1>Burger Shop</h1>
  </div>
</template>

<script>
export default {
  name: 'Banner'
}
</script>

<style scoped>
  #main-banner {
    display: flex;
    flex-shrink: 0;
    align-items: center;
    height: 60vh;
    min-height: 500px;
    background: url('/img/burger.jpg') center 70% / cover no-repeat;
    padding: 50px 5%;
  }

  #main-banner h1 {
    font-size: clamp(60px, 8vw, 120px);
    color: var(--color-white);
    margin-bottom: 0;
  }
</style>