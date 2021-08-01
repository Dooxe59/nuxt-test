<template>
  <div class="user-posts">
    <!-- Manage loading -->
    {{ userPosts }}
  </div> 
</template>

<script lang="ts">
import axios from 'axios';

import Vue from 'vue';
import { RequestStatus } from '~/enums/RequestStatus.enum';
import { ResponseData } from '~/models/ResponseData';
import { UserPost } from '~/models/users/UserPost.api.model';

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
      userPosts: {} as UserPost[],
    }
  },
  watch: {
    userId(): void {
      this.loadUserPosts();
    },
  },
  computed: {
    loadUserPostsUrl(): string {
      // TODO: const baseURI
      return `https://jsonplaceholder.typicode.com/posts?userId=${this.userId}`;
    },
  },
  methods: {
    loadUserPosts(): void {
      this.requestStatus = RequestStatus.LOADING;
      axios
        .get(this.loadUserPostsUrl)
        .then((response: ResponseData) => {
          this.userPosts = response.data;
          this.requestStatus = RequestStatus.SUCCESS;
        })
        .catch((e: Error) => {
          this.requestStatus = RequestStatus.ERROR;
          this.requestError = e;
        });
    },
  },
  mounted() {
    this.loadUserPosts();
  }
});

</script>

<style lang="scss" scoped>
@import "styles/variables.scss";
  .user-posts {

  }
</style>