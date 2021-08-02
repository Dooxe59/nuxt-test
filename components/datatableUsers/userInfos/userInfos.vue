<template>
  <div class="advanced-user-infos">
    <div v-if="isRequestStatusSuccess" class="user-infos-container">
      <div v-if="!advancedUserInfos" class="no-user-infos">
        No data available
      </div>
      <div v-else class="advanced-user-infos-blocs">
        <General-infos :user-infos="advancedUserInfos"/>
        <Address-infos :address-infos="advancedUserInfos.address"/>
        <Company-infos :company-infos="advancedUserInfos.company"/>
      </div>
    </div>
    <div v-else-if="isRequestStatusLoading" class="loading-users-infos-progress">
      Loading user general informations in progress ...
    </div>
    <div v-else-if="isRequestStatusError && requestStatusError" class="loading-users-infos-error">
      An error has occurred: {{ requestStatusError }}
    </div>
  </div> 
</template>

<script lang="ts">
import axios from 'axios';

import Vue from 'vue';
import { RequestStatus } from '~/enums/RequestStatus.enum';
import { ResponseData } from '~/models/ResponseData';
import { User } from '~/models/users/User.api.model';
import GeneralInfos from './generalInfos.vue';
import AddressInfos from './addressInfos.vue';
import CompanyInfos from './companyInfos.vue';

export default Vue.extend({
  components: {
    AddressInfos,
    CompanyInfos,
    GeneralInfos,
  },
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
    isRequestStatusLoading(): boolean {
      return this.requestStatus === RequestStatus.LOADING;
    },
    isRequestStatusSuccess(): boolean {
      return this.requestStatus === RequestStatus.SUCCESS;
    },
    isRequestStatusError(): boolean {
      return this.requestStatus === RequestStatus.ERROR;
    },
    requestStatusError(): string {
      return this.requestError?.message;
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
    .advanced-user-infos-blocs {
      @media screen and (min-width: $breakpoint-tablet-max) and (max-width: $breakpoint-desktop-min) {
        margin: 0 15%;
      }

      @media screen and (min-width: $breakpoint-desktop-min) {
        margin: 0 30%;
      }
    }
  }
</style>