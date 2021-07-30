<template>
  <div class="index-container">
    <NuxtLink to="/images" class="link">
      Test images
    </NuxtLink>
    <NuxtLink to="/datatableUsers" class="link">
      Test Datatable users
    </NuxtLink>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({})
</script>

<style lang="scss" scoped>
@import "./styles/variables.scss";

  .index-container {
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;

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
  }
</style>
