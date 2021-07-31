<template>
  <div class="images-container">
    <div class="exercice-description">
      <span class="test-description">
        The goal of this test is to display 10 random images by using the Flickity component and Picsum APIs
      </span>
    </div>
    <carousel v-if="isRequestStatusSuccess" class="carousel">
      <img 
        v-for="image in images"
        class="carousel-cell"
        :key="image.id"
        :src="image.download_url"
        :alt="`Image created by ${image.author}`" />
    </carousel>
    <div v-else-if="isRequestStatusLoading" class="loading-images-progress">
      Loading in progress ...
    </div>
    <div v-else-if="isRequestStatusError && requestStatusError" class="loading-images-error">
      An error has occurred: {{ requestStatusError }}
    </div>
  </div>
</template>

<script lang="ts">
// Create plugins or mixin to manage api methods
import axios from 'axios';

// Global export / import models
import { ImageDto } from '~/models/images/Image.dto.model';
import { ResponseData } from 'models/ResponseData';
import { RequestStatus } from '~/enums/RequestStatus.enum';

import { generateRandomNumber } from '~/utils/generateRandomNumber';
import carousel from '~/components/shared/carousel.vue';


export default {
  components: { 
    carousel
  },
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
    const Flickity = require('flickity');
    this.loadImages(); 
  }
}
</script>

<style lang="scss" scoped>
@import 'flickity/dist/flickity.min.css';
@import "styles/variables.scss";

  .images-container {
    height: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;

    .exercice-description {
      width: 100%;
      margin: 0 $spacing-sm;
    }

    .carousel {
      width: 100%;
      margin-top: 30px;
      height: 150px;

      @media screen and (min-width: $breakpoint-tablet-max) and (max-width: $breakpoint-desktop-min) {
        height: 225px;
      }

      @media screen and (min-width: $breakpoint-desktop-min) {
        height: 300px;
        width: 80%;
      }

      .carousel-cell {
        width: 50%;
        margin-right: $spacing;
        border-radius: $radius;
        height: 150px;

        @media screen and (min-width: $breakpoint-tablet-max) and (max-width: $breakpoint-desktop-min) {
        height: 225px;
      }

        @media screen and (min-width: $breakpoint-desktop-min) {
          height: 300px;
        }
      }
    }

    .loading-images-progress, .loading-images-error {
      display: flex;
      flex: 1;
      align-items: center;
    }
  }
  
</style>
