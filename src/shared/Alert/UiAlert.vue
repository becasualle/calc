<template>
  <div :class="['alert', classes]">
    <p class="text">
      <slot />
    </p>
  </div>
</template>

<script setup lang="ts">
import { computed, toRefs } from 'vue';

interface Props {
  success?: boolean;
  error?: boolean;
}

const props = withDefaults(defineProps<Props>(), { success: false, error: false });

const { success, error } = toRefs(props);

const classes = computed(() => ({
  'alert--success': success.value === true,
  'alert--error': error.value === true,
}));
</script>

<style lang="scss" scoped>
.alert {
  margin: 1rem 0;
  padding: 1em;
  border-radius: 0.375rem;
  border: 1px solid lightgray;

  &--success {
    background-color: #d1e7dd;
    border: 1px solid #badbcc;
    .text {
      color: #0f5132;
    }
  }

  &--error {
    background-color: #f8d7da;
    border: 1px solid #f5c2c7;
    .text {
      color: #842029;
    }
  }
}
</style>
