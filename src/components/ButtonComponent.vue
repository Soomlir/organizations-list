<script>
export default {
  name: "ButtonComponent",
  props: {
    type: {
      type: String,
      default: 'button',
    },
    secondary: {
      type: Boolean,
      default: false,
    }
  },
  emits: ['click'],
};
</script>

<template>
  <button
    class="button"
    :class="{ 'button--secondary': secondary }"
    @click="$emit('click');"
    :type
  >
    <slot />
  </button>
</template>
