<template>
  <div class="images-container">
    <span class="test-description">
      The goal of this test is to display 10 random images by using the Flickity component and Picsum APIs
    </span>
    {{ images }}
    <carousel>
      <div class="carousel-cell">A</div>
      <div class="carousel-cell">B</div>
      <div class="carousel-cell">C</div>
      <div class="carousel-cell">D</div>
      <div class="carousel-cell">E</div>
    </carousel>
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
  components: { carousel },
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
    const Flickity = require('flickity');
  }
}
</script>

<style lang="scss" scoped>
@import 'flickity/dist/flickity.min.css';

  .images-container {
   
  }

  .carousel-cell {
    width: 66%;
    height: 200px;
    margin-right: 10px;
    background: #8C8;
    border-radius: 5px;
  }
</style>
