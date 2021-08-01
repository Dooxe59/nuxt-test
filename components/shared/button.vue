<template>
  <V-btn 
    depressed 
    :color="color"
    @click.stop="onClickButton"> 
    {{ label }}
  </V-btn>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  props: {
    color: {
      type: String, 
    },
    label: {
      type: String, 
      required: true,
    }
  },
  methods: {
    onClickButton() {
      this.$emit('click');
    }
  }
});

</script>

<style lang="scss" scoped>
@import "styles/variables.scss";

</style>