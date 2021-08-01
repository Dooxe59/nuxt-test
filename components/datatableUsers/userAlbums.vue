<template>
  <div class="user-albums">
    <!-- Manage loading -->
    {{ userAlbums }}
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
      userAlbums: {} as UserAlbum[],
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
  .user-albums {

  }
</style>