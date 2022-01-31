<template>
  <Banner />
  <main class="main-container">
    <h1>Monte seu Burger:</h1>
    <BurgerForm />
  </main>
</template>

<script>
// @ is an alias to /src
import Banner from '@/components/Banner.vue';
import BurgerForm from '@/components/BurgerForm.vue';

export default {
  name: 'Home',
  components: {
    Banner,
    BurgerForm
  }
}
</script>
