<template>
  <ui-wrapper>
    <build-calc msg="Welcome to Your Vue.js + TypeScript App" />
  </ui-wrapper>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import BuildCalc from '../widgets/Calc.vue';
import UiWrapper from '../shared/Wrapper/UiWrapper.vue';

export default defineComponent({
  name: 'App',
  components: {
    BuildCalc,
    UiWrapper,
  },
});
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}

label {
  cursor: pointer;
}

input,
textarea,
select {
  display: block;
  width: 100%;
  padding: 0.375rem 0.75rem;

  font-family: 'Hauora', sans-serif;
  font-size: 1rem;

  background-color: #fff;
  border: 1px solid #d5d8de;
  border-radius: 0.25em;
}
</style>
