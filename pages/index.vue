<template>
  <div class="index-container">
    <NuxtLink to="/test1" class="link">
      Test 1
    </NuxtLink>
    <NuxtLink to="/test2" class="link">
      Test 2
    </NuxtLink>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({})
</script>

<style lang="scss" scoped>
  .index-container {
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;

    .link {
      border: 1px solid #47b27a;
      background-color: #47b27a;  
      margin: 10px;
      padding: 5px 15px;
      color: white;  
      border-radius: 5px;
      text-decoration: none;

      &:hover {
        opacity: 0.8;
      }
    }
  }
</style>
