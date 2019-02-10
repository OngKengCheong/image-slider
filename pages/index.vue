<template>
  <v-layout
    column
    justify-center
    align-center
  >
    <v-flex
      xs12
      sm8
      md6
      class="slider-wrapper"
    >
      <a @click="prev" class="slide-control slide-prev"></a>
        
      <transition-group name='fade' tag='div' v-if="pageReady" class="slider-content">
        <v-flex
          class="slide-item"
          v-for="number in [currentNumber]"
          :key="number"
          xs12
          d-flex
        >
          <v-card flat tile class="d-flex">
            <v-img
              class="slide-image"
              :src="currentImage"
              :lazy-src="currentImage"
            >
              <v-layout
                slot="placeholder"
                fill-height
                align-center
                justify-center
                ma-0
              >
                <v-progress-circular indeterminate color="grey lighten-5"></v-progress-circular>
              </v-layout>
            </v-img>
          </v-card>
        </v-flex>
      </transition-group>

      <a @click="next" class="slide-control slide-next"></a>
    </v-flex>
  </v-layout>
</template>

<script>
import Logo from '~/components/Logo.vue'
import VuetifyLogo from '~/components/VuetifyLogo.vue'
import axios from 'axios';

export default {
  data: function() {
    return {
      pageReady: false,
      images: [],
      currentNumber: 0,
    };
  },    
  computed: {
    currentImage: function() {
      if (this.pageReady && this.images.length) {
        return this.images[this.currentNumber];
      }
      return null;
    }
  },
  methods: {
    next: function() {
        this.currentNumber = (this.currentNumber + 1) % this.images.length;
    },
    prev: function() {
        this.currentNumber = (this.currentNumber - 1 + this.images.length) % this.images.length;
    }
  },
  components: {
    Logo,
    VuetifyLogo
  },
  mounted: function() {
    axios.get('http://www.mocky.io/v2/5b753d5f2e00006900535f6c').then(({data}) => {
      const {
        images
      } = data;
      this.pageReady = true;
      this.images = images || [];
      console.log(this.images);
    })
  }
}
</script>

<style>
.slider-wrapper {
  min-width: 640px;
  min-height: 480px;
  flex: 1;
  position: relative;
}
.slide-control {
  position: absolute;
  top: 50%;
}
.slide-control:after {
  content: '';
  display: block;
  width: 25px;
  height: 25px;
  background: transparent;
  border-style: solid;
  border-width: 2px;
}
.slide-prev {
  left: -40px;
}
.slide-prev:after {
  border-color: #fff transparent transparent #fff;
  transform: rotate(-45deg);
}
.slide-next {
  right: -40px;
}
.slide-next:after {
  border-color: #fff #fff transparent transparent;
  transform: rotate(45deg);
}
.slider-content {
  width: 100%;
  height: 100%;
}
.slide-item {
  width: 100%;
  height: 100%;
}
.slide-image {
}
.fade-enter-active, .fade-leave-active {
  transition: all 0.8s ease;
  overflow: hidden;
  visibility: visible;
  opacity: 1;
  position: absolute;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
  visibility: hidden;
}
</style>