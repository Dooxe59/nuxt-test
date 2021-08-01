<template>
  <div class="users-infos-datatable">
    <V-app>
      <V-data-table
        class="elevation-1" 
        hide-default-footer
        :headers="headers"
        :items="users" 
        @click:row="selectDatatableRow"/>
      <V-dialog 
        width="500"
        v-model="showAdvancedUserInfos">
        <V-card>
          <V-card-title class="text-h5 grey lighten-2">
            Advanced user informations
          </V-card-title>
          <V-card-text>
            <!-- <User-infos :user-id="selectedUserId"/> -->
            <!-- <User-posts :user-id="selectedUserId"/> -->
            <!-- <User-todos :user-id="selectedUserId"/> -->
            <User-albums :user-id="selectedUserId"/>
          </V-card-text>
          <V-divider></V-divider>
          <V-card-actions>
            <V-spacer></V-spacer>
            <Button label="Close" @click="closeAdvancedUserInfos" />
          </V-card-actions>
        </V-card>
      </V-dialog>
    </V-app>
  </div> 
</template>

<script lang="ts">
import { UserDto } from '~/models/users/User.dto.model';
import Button from '~/components/shared/button.vue';
import UserInfos from './userInfos.vue';
import UserPosts from './userPosts.vue';
import UserTodos from './userTodos.vue';
import UserAlbums from './userAlbums.vue';

import Vue from 'vue';

export default Vue.extend({
  components: {
    Button,
    UserInfos,
    UserPosts,
    UserTodos,
    UserAlbums,
  },
  props: {
    users: {
      type: Array, 
      required: true,
    }
  },
  watch: {
    showAdvancedUserInfos() {
      if(!this.showAdvancedUserInfos) {
        this.clearSelectedUser();
      }
    }
  },
  data() {
    return {
      headers: [
        {
          text: 'User id',
          value: 'id',
          sortable: false,
        },
        { 
          text: 'Username', 
          value: 'username',
          sortable: false,
        },
        { 
          text: 'Email', 
          value: 'email' ,
          sortable: false,
        },
        { 
          text: 'Company name', 
          value: 'companyName',
          sortable: false,
        },
      ],
      showAdvancedUserInfos: false,
      selectedUser: {} as UserDto,
    }
  },
  computed: {
    selectedUserId() : String {
      return `${this.selectedUser.id}`;
    },
  },
  methods: {
    selectDatatableRow(event: UserDto) : void {
      this.selectedUser = event;
      this.showAdvancedUserInfos = true;
    },
    closeAdvancedUserInfos(): void {
      this.showAdvancedUserInfos = false;
    },
    clearSelectedUser(): void {
      this.selectedUser = {} as UserDto;
    }
  }
});

</script>

<style lang="scss" scoped>
@import "styles/variables.scss";
  .users-infos-datatable {
    margin-top: 30px;
  }
</style>