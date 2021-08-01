<template>
  <div class="user-todos">
    <div v-if="isRequestStatusSuccess" class="user-todos-container">
      <div v-if="userTodos.length === 0" class="no-user-todos">
        No data available
      </div>
      <div 
        v-else
        v-for="userTodo in userTodos"
        :key="userTodo.id">
        <v-card class="user-todos-card" elevation="2">
          <div class="text-infos-container">
            <div class="key-value-container">
              <span class="key">
                Id:
              </span>
              <span class="value">
                {{userTodo.id}}
              </span>
            </div>
            <div class="key-value-container">
              <span class="key">
                Title:
              </span>
              <span class="value">
                {{userTodo.title}}
              </span>
            </div>
          </div>
          <div class="completed-task-infos">
            <v-icon   
              v-if="userTodo.completed" 
              title="User has completed task"
              aria-hidden="false">
              mdi-chevron-down-box
            </v-icon>
          </div>
        </v-card>
      </div>   
    </div>
    <div v-else-if="isRequestStatusLoading" class="loading-users-todos-progress">
      Loading user todos in progress ...
    </div>
    <div v-else-if="isRequestStatusError && requestStatusError" class="loading-users-todos-error">
      An error has occurred: {{ requestStatusError }}
    </div>
  </div> 
</template>

<script lang="ts">
import axios from 'axios';

import Vue from 'vue';
import { RequestStatus } from '~/enums/RequestStatus.enum';
import { ResponseData } from '~/models/ResponseData';
import { UserTodo } from '~/models/users/UserTodo.api.model';

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
      userTodos: {} as UserTodo[],
    }
  },
  watch: {
    userId(): void {
      this.loadUserTodos();
    },
  },
  computed: {
    loadUserTodosUrl(): string {
      return `https://jsonplaceholder.typicode.com/todos?userId=${this.userId}`;
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
    loadUserTodos(): void {
      this.requestStatus = RequestStatus.LOADING;
      axios
        .get(this.loadUserTodosUrl)
        .then((response: ResponseData) => {
          this.userTodos = response.data;
          this.requestStatus = RequestStatus.SUCCESS;
        })
        .catch((e: Error) => {
          this.requestStatus = RequestStatus.ERROR;
          this.requestError = e;
        });
    },
  },
  mounted() {
    this.loadUserTodos();
  }
});

</script>

<style lang="scss" scoped>
@import "styles/variables.scss";
  .user-todos-container {

    @media screen and (min-width: $breakpoint-tablet-max) and (max-width: $breakpoint-desktop-min) {
      margin: 0 15%;
    }

    @media screen and (min-width: $breakpoint-desktop-min) {
      margin: 0 30%;
    }

    .user-todos-card {
      display: flex;
      margin-bottom: $spacing;
      padding: $spacing;

      .text-infos-container {
        flex: 1;
      }

      .completed-task-infos {
        margin: $spacing;
      }
    }
  }
</style>