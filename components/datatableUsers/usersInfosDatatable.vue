<template>
  <div class="users-infos-datatable">
    <V-data-table
      class="elevation-1" 
      hide-default-footer
      :headers="headers"
      :items="users" 
      @click:row="selectDatatableRow"/>
  </div> 
</template>

<script lang="ts">
import Vue from 'vue';
import { UserDto } from '~/models/users/User.dto.model';

export default Vue.extend({
  props: {
    users: {
      type: Array, 
      required: true,
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
    }
  },
  methods: {
    selectDatatableRow(event: UserDto) : void {
      console.log('ok', event)
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