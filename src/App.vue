<template>
  <div id="app">
    <b-container>
      <formValidation />
    </b-container>
  </div>
</template>

<script>
import formValidation from '@/components/formValidation.vue';

export default {
  name: 'App',
  components: {
    formValidation,
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 60px;
}
</style>
