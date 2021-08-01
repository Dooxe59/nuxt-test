<template>
  <div class="users-infos-datatable">
    <V-app>
      <V-data-table
        class="elevation-3" 
        hide-default-footer
        :headers="headers"
        :items="users" 
        @click:row="selectDatatableRow"/>
      <V-dialog 
        width="500"
        fullscreen
        hide-overlay
        v-model="showAdvancedUserInfos">
        <V-card>
          <V-card-title class="advanced-users-infos-title-modal">
            <span class="title">User informations</span>
            <Button 
              label="Close"
              @click="closeAdvancedUserInfos" />
          </V-card-title>
          <V-card-text>
            <V-tabs color="green" v-model="currentTab">
              <V-tabs-slider color="green"></v-tabs-slider>
              <V-tab
                v-for="tab in tabs"
                :key="tab">
                {{ tab }}
              </V-tab>
            </V-tabs>
            <User-infos v-show="showUserInfos" :user-id="selectedUserId"/>
            <User-posts v-show="showUserPosts" :user-id="selectedUserId"/>
            <User-todos v-show="showUserTodos" :user-id="selectedUserId"/>
            <User-albums v-show="showUserAlbums" :user-id="selectedUserId"/>
          </V-card-text>
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
      tabs: [
        'Infos', 'Posts', 'Todos', 'Albums',
      ],
      currentTab: 0,
      showAdvancedUserInfos: false,
      selectedUser: {} as UserDto,
    }
  },
  computed: {
    selectedUserId(): string {
      return `${this.selectedUser.id}`;
    },
    showUserInfos(): boolean {
      return this.currentTab === 0;
    },
    showUserPosts(): boolean {
      return this.currentTab === 1;
    },
    showUserTodos(): boolean {
      return this.currentTab === 2;
    },
    showUserAlbums(): boolean {
      return this.currentTab === 3;
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

    .advanced-users-infos-title-modal{
      color: $color-white;
      background-color: $color-green;
      display: flex;
      align-items: center;

      .title {
        flex: 1;
      }
    }
  }
</style>