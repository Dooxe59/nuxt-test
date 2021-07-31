<template>
  <div class="index-container">
    <Link to="/images" label="Test images"/>
    <Link to="/datatableUsers" label="Test Datatable users"/>
  </div>
</template>

<script lang="ts">
import Link from "../components/shared/link.vue";

export default {
  components: {
    Link,
  }
}
</script>

<style lang="scss" scoped>
@import "styles/variables.scss";

  .index-container {
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
  }
</style>
