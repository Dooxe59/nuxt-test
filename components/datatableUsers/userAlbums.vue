<template>
  <div class="user-albums">
    <div v-if="isRequestStatusSuccess" class="user-albums-container">
      <div v-if="userAlbums.length === 0" class="no-user-albums">
        No data available
      </div>
      <div 
        v-else
        v-for="userAlbum in userAlbums"
        :key="userAlbum.id">
        <v-card class="user-albums-card" elevation="2">
          <div class="key-value-container">
            <span class="key">
              Id:
            </span>
            <span class="value">
              {{userAlbum.id}}
            </span>
          </div>
          <div class="key-value-container">
            <span class="key">
              Title:
            </span>
            <span class="value">
              {{userAlbum.title}}
            </span>
          </div>
        </v-card>
      </div>   
    </div>
    <div v-else-if="isRequestStatusLoading" class="loading-users-albums-progress">
      Loading user albums in progress ...
    </div>
    <div v-else-if="isRequestStatusError && requestStatusError" class="loading-users-albums-error">
      An error has occurred: {{ requestStatusError }}
    </div>
  </div> 
</template>

<script lang="ts">
import axios from 'axios';

import Vue from 'vue';
import { RequestStatus } from '~/enums/RequestStatus.enum';
import { ResponseData } from '~/models/ResponseData';
import { UserAlbum } from '~/models/users/UserAlbum.api.model';

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
      userAlbums: [] as UserAlbum[],
    }
  },
  watch: {
    userId(): void {
      this.loadUserAlbums();
    },
  },
  computed: {
    loadUserAlbumsUrl(): string {
      return `https://jsonplaceholder.typicode.com/albums?userId=${this.userId}`;
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
    loadUserAlbums(): void {
      this.requestStatus = RequestStatus.LOADING;
      axios
        .get(this.loadUserAlbumsUrl)
        .then((response: ResponseData) => {
          this.userAlbums = response.data;
          this.requestStatus = RequestStatus.SUCCESS;
        })
        .catch((e: Error) => {
          this.requestStatus = RequestStatus.ERROR;
          this.requestError = e;
        });
    },
  },
  mounted() {
    this.loadUserAlbums();
  }
});

</script>

<style lang="scss" scoped>
@import "styles/variables.scss";
  .user-albums-container {

    @media screen and (min-width: $breakpoint-tablet-max) and (max-width: $breakpoint-desktop-min) {
      margin: 0 15%;
    }

    @media screen and (min-width: $breakpoint-desktop-min) {
      margin: 0 30%;
    }

    .user-albums-card {
      margin-bottom: $spacing;
      padding: $spacing;
    }
  }
</style>