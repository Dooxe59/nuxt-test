<template>
  <div class="user-posts">
    <div v-if="isRequestStatusSuccess" class="user-posts-container">
      <div v-if="userPosts.length === 0" class="no-user-posts">
        No data available
      </div>
      <div 
        v-else
        v-for="userPost in userPosts"
        :key="userPost.id">
        <v-card class="user-post-card" elevation="2">
          <div class="card-header">
            <span class="post-title">
              {{userPost.title}}
            </span>
            <span class="post-id" :title="`Post id ${userPost.id}`">
              {{userPost.id}}
            </span>
          </div>
          <div class="post-body">
            {{userPost.body}}
          </div>
        </v-card>
      </div>   
    </div>
    <div v-else-if="isRequestStatusLoading" class="loading-users-posts-progress">
      Loading user posts in progress ...
    </div>
    <div v-else-if="isRequestStatusError && requestStatusError" class="loading-users-posts-error">
      An error has occurred: {{ requestStatusError }}
    </div>
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

  .user-posts-container {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
    margin-top: 30px;

    .user-post-card {
      width: 250px;
      height: 200px;
      margin-bottom: 20px;

      @media screen and (min-width: $breakpoint-desktop-min) {
        width: 400px;
        height: 300px;
      }

      .card-header {
        display : flex;
        padding: $spacing-sm 0;
        color: $color-white;
        background: $color-green;
        height: 75px;

        .post-title {
          flex: 1;
          margin: 0 $spacing-sm;
        }

        .post-id {
          margin: $spacing-sm;
          background-color: $color-white;
          color: $color-green;
          border-radius: 10px;
          flex-shrink: 0;
          height: 30px;
          width: 30px;
          display: flex;
          align-items: center;
          justify-content: center;
        }
      }

      .post-body {
        padding: $spacing;
        height: 125px;
        overflow: auto;

        @media screen and (min-width: $breakpoint-desktop-min) {
          height: 225px;
        }
      }
    }
  }
</style>