<template>
  <Navbar :logo="logo_src" :alt="app_name" />
  <router-view/>
  <Footer />
</template>

<script>
import Navbar from '@/components/Navbar.vue'
import Footer from '@/components/Footer.vue'

export default {
  components: {
    Navbar,
    Footer
  },
  data() {
    return {
      logo_src: './img/logo.png',
      app_name: 'Burger Shop'
    }
  }
}
</script>

<style>
  :root {
    --color-primary: #673ab7;
    --color-primary-accent: #8e63da;
    --color-secondary: #fccf3f;
    --color-secondary-accent: #fce93f;
    --color-white: #fff;
    --color-black: #000;
    --color-gray: #efefef;
    --font-default: 'Roboto Condensed', sans-serif;
    --font-alt: 'Lobster', cursive;
    --transition-default: all .2s ease-out;
  }

  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
    font-family: var(--font-default);
  }

  ::selection {
    background-color: var(--color-secondary);
    color: var(--color-primary);
  }

  html, 
  body {
    height: 100%;
  }

  #app {
    display: flex;
    height: 100%;
    flex-direction: column;
  }

  h1,
  h2,
  h3,
  h4,
  h5,
  h6 {
    font-family: var(--font-alt);
    color: var(--color-primary);
    margin-bottom: 1em;
  }

  h1 {
    font-size: 40px;
    text-align: center;
    text-decoration: underline;
    text-decoration-color: var(--color-secondary);
  }

  .main-container {
    flex: 2;
    background-color: #fafafa;
    padding: clamp(40px, 7vw, 80px) 5%;
  }
</style>
