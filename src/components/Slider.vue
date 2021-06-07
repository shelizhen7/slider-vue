<template>
  <div style="height: 15rem; position: relative" ref="sliderRef">
    <div>
      <div
        class="image-content"
        :style="{
          transform: `translateX(${slideX}px)`,
          transition: `transform 1s ease`,
        }"
      >
        <div
          v-for="(item, index) in list"
          class="img"
          :style="`background-image:url(${item})`"
        ></div>
        <!-- <div v-for="(item,index) in list" class="img">
            <img width="110" height="auto" :src="`${item}`" alt="">
        </div> -->
        <!-- <div class="img"></div>
        <div class="img"></div>
        <div class="img"></div>
        <div class="img"></div> -->
      </div>
    </div>

    <div v-if="isDisplay" class="slider-btn slider-btn-left" @click="prev()">
      <!-- <em class="slider-icon slider-icon-left"></em> -->
      <div class="triangle-left"></div>
    </div>
    <div v-if="!isDisplay" class="slider-btn slider-btn-right" @click="next()">
      <div class="triangle-right"></div>
    </div>
  </div>
</template>
<script>
export default {
  name: "slider",
  props: {
    list: {
      type: Array,
    },
  },
  data() {
    return {
      slideX: "",
      cur: 0,
      len: 2,
      isDisplay: false,
    };
  },
  methods: {
    next() {
      this.cur++;
      if (this.cur == this.len - 1) {
        this.isDisplay = true;
      }
      this.slideX = -this.cur * this.$refs.sliderRef.offsetWidth;
    },
    prev() {
      this.cur--;
      if (this.cur == 0) {
        this.isDisplay = false;
      }
      this.isDisplay = false;
      this.slideX = this.cur * this.$refs.sliderRef.offsetWidth;
    },
  },
};
</script>
<style scoped>
.image-content {
  padding: 0.625rem;
  height: 10rem;
  overflow: hidden;
  /* display: flex; */
  /* display:inline-block; */
  float: left;
  /* width:100%; */
  white-space: nowrap;
}
.img {
  background-color: gray;
  /* flex-grow: 1; */
  width: 7rem;
  border-radius: 5px;
  margin: 5px;
  display: inline-block;
  height: 100%;
  background-size: contain;
}
.slider-btn {
  position: absolute;
  top: 5rem;
  z-index: 999;
  width: 1.875rem;
  height: 1.875rem;
  border: none;
  background: transparent;
  outline: 0;
  cursor: pointer;
  border-radius: 50%;
  background-color: rgba(245, 128, 36, 0.4);
}
.slider-icon-left {
  transform: rotate(45deg);
}
.slider-icon-right {
  transform: rotate(225deg);
}
.slider-icon {
  display: inline-block;
  width: 0.9rem;
  height: 0.9rem;
  border-left: 2px solid rgba(255, 255, 255, 0.6);
  border-bottom: 2px solid rgba(255, 255, 255, 0.6);
}
.slider-btn-left {
  left: 1rem;
}

.slider-btn-right {
  right: 1rem;
}

.triangle-left {
  margin: 5px -3px;
  float: left;
  width: 0;
  height: 0;
  border-width: 10px;
  border-style: solid;
  border-color: transparent #fff transparent transparent;
}
.triangle-right {
  margin: 5px 12px;
  float: left;
  width: 0;
  height: 0;
  border-width: 10px;
  border-style: solid;
  border-color: transparent transparent transparent #fff;
}
</style>