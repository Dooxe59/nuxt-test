<template>
  <div class="carousel">
    <slot />
  </div>
</template>

<script>
export default {
  mounted() {
    this.init();
  },
  data() {
    return {
      flickityDefaultOptions: {
        prevNextButtons: true,
        pageDots: true,
        wrapAround: true,
        autoPlay: true,
      }
    }
  },
  methods: {
    init() {
      const FlickityInstance = require('flickity');
      this.$flickity = new FlickityInstance(this.$el, this.flickityDefaultOptions);
      this.$emit('init', this.$flickity);
    },
  },
  beforeDestroy() {
    this.$flickity.destroy();
    this.$flickity = null;
  },
};
</script>

<style src='flickity/dist/flickity.css'></style>