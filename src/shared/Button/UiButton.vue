<template>
  <button :class="['ui-button', classes]"><slot /></button>
</template>

<script setup lang="ts">
import { computed, toRefs } from 'vue';

interface Props {
  theme?: string;
}

const props = withDefaults(defineProps<Props>(), {
  theme: 'default',
});

const { theme } = toRefs(props);

const classes = computed(() => ({
  'ui-button--hollow': theme.value === 'hollow',
}));
</script>

<style lang="scss" scoped>
.ui-button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  background-color: #000;
  border-radius: 0.5em;
  color: #fff;
  padding: 1em;
  line-height: 1.25;
  border: 1px solid #000;
  transition: all 0.4s ease;
  //gap: var(--sm-gap);

  &:hover {
    background-color: transparent;
    color: #000;
  }

  &:active {
    opacity: 0.5;
  }

  &--hollow {
    background-color: transparent;
    color: #000;
    &:hover {
      background-color: #000;
      color: #fff;
    }
  }
}
</style>
