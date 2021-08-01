<template>
  <div class="advanced-user-infos">
    <!-- Manage loading -->
    {{ advancedUserInfos }}
  </div> 
</template>

<script lang="ts">
import axios from 'axios';

import Vue from 'vue';
import { RequestStatus } from '~/enums/RequestStatus.enum';
import { ResponseData } from '~/models/ResponseData';
import { User } from '~/models/users/User.api.model';

export default Vue.extend({
  props: {
    userId: {
      type: String, 
      required: true,
    }
  },
  data() {
    return {
      requestStatus: RequestStatus.DEFAULT as RequestStatus, 
      requestError: {} as Error, 
      advancedUserInfos: {} as User,
    }
  },
  computed: {
    loadAdvancedUserInfosUrl(): string {
      return `https://jsonplaceholder.typicode.com/users/${this.userId}`;
    },
  },
  methods: {
    loadAdvancedUserInfos(): void {
      this.requestStatus = RequestStatus.LOADING;
      axios
        .get(this.loadAdvancedUserInfosUrl)
        .then((response: ResponseData) => {
          this.advancedUserInfos = response.data;
          this.requestStatus = RequestStatus.SUCCESS;
        })
        .catch((e: Error) => {
          this.requestStatus = RequestStatus.ERROR;
          this.requestError = e;
        });
    },
  },
  mounted() {
    this.loadAdvancedUserInfos();
  }
});

</script>

<style lang="scss" scoped>
@import "styles/variables.scss";
  .advanced-user-infos {

  }
</style>