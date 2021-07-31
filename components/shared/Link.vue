<template>
    <NuxtLink :to="to" class="link">
      {{ label }}
    </NuxtLink>
</template>

<script lang="ts">

export default {
  props: {
    to: {
      type: String,
      required: true,
    },
    label: {
      type: String,
      required: true,
    }
  },
};
</script>


<style lang="scss" scoped>
@import "./styles/variables.scss";

  .link {
    border: 1px solid $color-green;
    background-color: $color-green;  
    margin: $spacing;
    padding: $spacing-sm $spacing-md;
    color: $color-white;  
    border-radius: $radius;
    text-decoration: none;

    &:hover {
      opacity: 0.8;
    }
  }
</style>
