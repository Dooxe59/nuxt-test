<template>
  <div class="datatable-users-container">
    <div class="exercice-description">
      <span class="test-description">
        The goal of this test is to create a basic dashboard which display a data table of users and allow you to access
        to a user sheet which display advanced infos.
      </span>
    </div>
    <div v-if="isRequestStatusSuccess" class="users-infos">
      {{ users }}
    </div>
    <div v-else-if="isRequestStatusLoading" class="loading-users-progress">
      Loading user informations in progress ...
    </div>
    <div v-else-if="isRequestStatusError && requestStatusError" class="loading-users-error">
      An error has occurred: {{ requestStatusError }}
    </div>
  </div> 
</template>

<script lang="ts">
import axios from 'axios';

import { UserDto } from '~/models/users/User.dto.model';
import { User } from '~/models/users/User.api.model';
import { ResponseData } from 'models/ResponseData';
import { RequestStatus } from '~/enums/RequestStatus.enum';

import  Vue from 'vue';

export default Vue.extend({
  data() {
    return {
      requestStatus: RequestStatus.DEFAULT as RequestStatus, 
      requestError: {} as Error,
      users: [] as UserDto[],
      loadUserInfosUrl: 'https://jsonplaceholder.typicode.com/users',
    }
  },
  computed: {
    // Move to global mixin
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
    loadUsers(): void {
      this.requestStatus = RequestStatus.LOADING;
      axios
        .get(this.loadUserInfosUrl)
        .then((response: ResponseData) => {
          this.formatUsers(response.data);
          this.requestStatus = RequestStatus.SUCCESS;
        })
        .catch((e: Error) => {
          this.requestStatus = RequestStatus.ERROR;
          this.requestError = e;
        });
    },
    formatUsers(userList: User[]): void {
      this.users = userList.map((user: User) => {
        return {
          id: user.id,
          username: user.username,
          email: user.email,
          companyName: user?.company?.name,
        }
      });
    }
  },
  mounted() {
    this.loadUsers(); 
  }
});

</script>

<style lang="scss" scoped>
@import "styles/variables.scss";

  .datatable-users-container {
    height: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;

    .exercice-description {
      width: 100%;
      margin: 0 $spacing-sm;
    }

    // Duplicated style, create generic methods and style 
    .loading-images-progress, .loading-images-error {
      display: flex;
      flex: 1;
      align-items: center;
    }
  }
</style>