<template>
  <div class="company-user-infos">
    <v-card class="user-company-infos-card" elevation="2">
      <v-card-title>
        <span class="text-h6 font-weight-light">Company</span>
      </v-card-title>
      <div class="key-value-container">
        <span class="key">
          Company name:
        </span>
        <span class="value">
          {{companyInfos.name}}
        </span>
      </div>
      <div class="key-value-container">
        <span class="key">
          Catch phrase:
        </span>
        <span class="value">
          {{companyInfos.catchPhrase}}
        </span>
      </div>
      <div class="key-value-container">
        <span class="key">
          BS:
        </span>
        <span class="value">
          {{companyInfos.bs}}
        </span>
      </div>
    </v-card>
  </div> 
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  props: {
    companyInfos: {
      type: Object, 
      required: true,
    }
  },
});

</script>

<style lang="scss" scoped>
@import "styles/variables.scss";
  .company-user-infos {
    .user-company-infos-card {
      margin-bottom: $spacing;
      padding: $spacing;
    }
  }
</style>