<template>
  <div class="container">
    <Header />
    <Win />
    <Social />
  </div>
</template>

<script>
import Header from './components/Header.vue';
import Win from './components/Win.vue';
import Social from './components/Social.vue';

export default {
  name: 'app',
  components: {
    Header,
    Win,
    Social
  }
}
</script>

<style>
@import './assets/css/bootstrap.min.css';
@import './assets/css/style.css';

</style>
