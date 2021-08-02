<template>
  <div class="general-user-infos">
    <!-- Must be generic (v-for new object mapped) -->
    <v-card class="user-infos-card" elevation="2">
      <div class="key-value-container">
        <span class="key">
          Id:
        </span>
        <span class="value">
          {{userInfos.id}}
        </span>
      </div>
      <div class="key-value-container">
        <span class="key">
          Name:
        </span>
        <span class="value">
          {{userInfos.name}}
        </span>
      </div>
      <div class="key-value-container">
        <span class="key">
          Username:
        </span>
        <span class="value">
          {{userInfos.username}}
        </span>
      </div>
      <div class="key-value-container">
        <span class="key">
          Email:
        </span>
        <span class="value">
          {{userInfos.email}}
        </span>
      </div>
      <div class="key-value-container">
        <span class="key">
          Phone:
        </span>
        <span class="value">
          {{userInfos.phone}}
        </span>
      </div>
      <div class="key-value-container">
        <span class="key">
          Website:
        </span>
        <span class="value">
          {{userInfos.website}}
        </span>
      </div>
    </v-card>
  </div> 
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  props: {
    userInfos: {
      type: Object, 
      required: true,
    }
  },
});

</script>

<style lang="scss" scoped>
@import "styles/variables.scss";
  .general-user-infos {
    .user-infos-card {
      margin-bottom: $spacing;
      padding: $spacing;
    }
  }
</style>