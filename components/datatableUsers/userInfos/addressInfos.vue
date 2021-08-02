<template>
  <div class="address-user-infos">
    <v-card class="user-address-infos-card" elevation="2">
      <v-card-title>
        <span class="text-h6 font-weight-light">Address</span>
      </v-card-title>
      <div class="key-value-container">
        <span class="key">
          Street:
        </span>
        <span class="value">
          {{addressInfos.street}}
        </span>
      </div>
      <div class="key-value-container">
        <span class="key">
          Suite:
        </span>
        <span class="value">
          {{addressInfos.suite}}
        </span>
      </div>
      <div class="key-value-container">
        <span class="key">
          City:
        </span>
        <span class="value">
          {{addressInfos.city}}
        </span>
      </div>
      <div class="key-value-container">
        <span class="key">
          Zipcode:
        </span>
        <span class="value">
          {{addressInfos.zipcode}}
        </span>
      </div>
      <div class="key-value-container">
        <span class="key">
          Position (lat / long):
        </span>
        <span class="value">
          {{addressInfos.geo.lat}} / {{addressInfos.geo.lng}}
        </span>
      </div>
    </v-card>
  </div> 
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  props: {
    addressInfos: {
      type: Object, 
      required: true,
    }
  },
});

</script>

<style lang="scss" scoped>
@import "styles/variables.scss";
  .address-user-infos {
    .user-address-infos-card {
      margin-bottom: $spacing;
      padding: $spacing;
    }
  }
</style>