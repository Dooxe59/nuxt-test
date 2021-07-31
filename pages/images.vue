<template>
  <div class="images-container">
    <span class="test-description">
      The goal of this test is to display 10 random images by using the Flickity component and Picsum APIs
    </span>
    <div>
      {{ images }}
    </div>
  </div> 
</template>

<script lang="ts">
// Create plugins or mixin to manage api methods
import axios from 'axios';

// Global export / import models
import { ImageDto } from 'models/images/image.dto.model';
import { ResponseData } from 'models/ResponseData';
import { RequestStatus } from '~/enums/RequestStatus';

import { generateRandomNumber } from '~/utils/generateRandomNumber';

export default {
  data() {
    return {
      requestStatus: RequestStatus.DEFAULT as RequestStatus, 
      requestError: {} as Error,
      images: [] as ImageDto[],
    }
  },
  computed: {
    loadImageUrl(): string {
      const randomNumber = generateRandomNumber(1,100);
      return `https://picsum.photos/v2/list?page=${randomNumber}&limit=10`;
    }
  },
  methods: {
    loadImages(): void {
      this.requestStatus = RequestStatus.LOADING;
      axios
        .get(this.loadImageUrl)
        .then((response: ResponseData) => {
          this.images = response.data;
          this.requestStatus = RequestStatus.SUCCESS;
        })
        .catch((e: Error) => {
          this.requestStatus = RequestStatus.ERROR;
          this.requestError = e;
        });
    }
  },
  mounted() {
    this.loadImages(); 
  }
}
</script>

<style lang="scss" scoped>
  .images-container {
   
  }
</style>
