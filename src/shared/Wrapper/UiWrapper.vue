<template>
  <div class="wrapper"><slot /></div>
</template>

<script>
export default {
  name: 'UiWrapper',
};
</script>

<script setup lang="ts"></script>

<style lang="scss" scoped>
.wrapper {
  width: 100%;
  max-width: 1264px;
  padding: 0 40px;
  margin-left: auto;
  margin-right: auto;

  @media screen and (max-width: 767px) {
    max-width: 680px;
    padding: 0 32px;
  }
}
</style>
