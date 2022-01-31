<template>
  <div id="nav">
    <router-link to="/" id="logo-link">
      <img :src="logo" :alt="alt" id="logo">
    </router-link>
    <router-link to="/">Home</router-link>
    <router-link to="/pedidos">Pedidos</router-link>
  </div>
</template>

<script>
export default {
  name: 'Navbar',
  props: ['logo', 'alt']
}
</script>

<style scoped>
  #nav {
    display: flex;
    align-items: center;
    background-color: var(--color-primary);
    border-bottom: 5px solid var(--color-secondary);
    padding: 15px 5%;
  }

  #nav a {
    color: var(--color-secondary);
    font-family: var(--font-alt);
    font-size: 22px;
    text-decoration: none;
    transition: var(--transition-default);
    text-shadow: 2px 2px 0 var(--color-black), 
                -2px -2px 0 var(--color-black), 
                -2px 2px 0 var(--color-black), 
                2px -2px 0 var(--color-black), 
                2px 0 0 var(--color-black), 
                -2px 0 0 var(--color-black),
                0 2px 0 var(--color-black),
                0 -2px 0 var(--color-black);
  }

  #nav a + a {
    margin-left: 2em;
  }

  #nav a:hover {
    color: var(--color-secondary-accent);
  }

  #logo-link {
    margin-right: auto;
  }

  #logo {
    width: 60px;
  }
</style>