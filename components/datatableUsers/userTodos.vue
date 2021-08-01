<template>
  <div class="user-todos">
    <!-- Manage loading -->
    {{ userTodos }}
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
  .user-todos {

  }
</style>