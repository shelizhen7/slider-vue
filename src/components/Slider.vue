<template>
  <div class="vue-slide" ref="sliderRef">
    <div
      class="content"
      :style="{
        transform: `translateX(${slideX}px)`,
        transition: `transform ${transition / 1000}s ease`,
      }"
      @touchstart="touchStart"
      @touchmove="touchMove"
      @touchend="touchEnd"
      @transitionend="transitionEnd"
    >
      <div><img :src="prevItem" alt="previous" /></div>
      <div>
        <img
          :src="displayImg"
          alt="current"
          @click="$emit('click', currentIndex)"
        />
      </div>
      <div><img :src="nextItem" alt="next" /></div>
    </div>
    <div v-if="spot" class="spot">
      <div
        v-for="(d, i) in list.length"
        :class="{
          cur: i === currentIndex,
        }"
        :key="i"
      />
    </div>
    <button class="slider-btn slider-btn-left" @click="go_prev">
      <em class="slider-icon slider-icon-left"></em>
    </button>
    <button class="slider-btn slider-btn-right" @click="go_next">
      <em class="slider-icon slider-icon-right"></em>
    </button>
  </div>
</template>
<script>
import image from "@/assets/loading.gif";
export default {
  name: "Slider",
  props: {
    list: {
      type: Array,
    },
    time: {
      default: 3000,
      type: Number,
    },
    speed: {
      default: 300,
      type: Number,
    },
    auto: {
      default: false,
      type: Boolean,
    },
  },
  data() {
    return {
      currentIndex: 0,
      slideX: 0,
      transition: 0,
      touch: {
        startX: 0,
        startY: 0,
        ifN: false,
        direction: false,
        timer: null,
        duration: 0,
        scrollDirection: false,
      },
      spot: true,
      type: "ease",
      clockwise: true,
      offsetWidth: "",
    };
  },
  mounted() {
    // if (this.auto) this.setTimer();
    this.offsetWidth = this.$refs.sliderRef.offsetWidth
  },
  computed: {
    prevItem() {
      const item = this.list[this.currentIndex - 1];
      return item ? item : this.list[this.list.length - 1];
    },
    nextItem() {
      const item = this.list[this.currentIndex + 1];
      return item ? item : this.list[0];
    },
    displayImg() {
      // console.log('displayImg----',this.currentIndex)
      if (this.list.length == 0) return image;
      return this.list[this.currentIndex];
    },
  },
  methods: {
    touchStart(e) {
      this.touch.startX = e.touches[0].clientX;
      this.touch.startY = e.touches[0].clientY;
      this.touch.ifN = true;
      this.touch.duration = new Date().getTime();
      window.clearInterval(this.touch.timer);
      this.transition = 0;
    },
    touchMove(e) {
      if (this.touch.ifN) {
        if (
          this.getDistance(e.touches[0].clientX, this.touch.startX) >=
          this.getDistance(e.touches[0].clientY, this.touch.startY)
        ) {
          this.touch.direction = true;
          e.currentTarget.style.transition = "";
        } else this.touch.direction = false;
        this.touch.ifN = false;
      } else {
        if (this.touch.direction) {
          e.preventDefault();
          this.slideX = e.touches[0].clientX - this.touch.startX;
        }
      }
    },
    touchEnd() {
      if (!this.touch.direction) return;
      const time = new Date().getTime() - this.touch.duration;
      if (
        time > 300 &&
        this.getDistance(this.slideX) < this.$refs.sliderRef.offsetWidth / 3
      ) {
        this.touch.scrollDirection = false;
        this.transition = this.speed * 0.8;
        this.slideX = 0;
      } else {
        if (this.slideX !== 0) {
          this.slideX > 0 ? this.go_prev() : this.go_next();
        }
      }
      this.setTimer();
    },
    getDistance(x, y = 0) {
      return parseInt((x - y).toString().replace("-", ""));
    },
    go_prev() {
      this.touch.scrollDirection = "prev";
      this.transition = this.speed;
      this.slideX = this.$refs.sliderRef.offsetWidth;
       this.slideX = this.$refs.sliderRef ? this.$refs.sliderRef.offsetWidth:this.offsetWidth;
    },
    go_next() {
      this.touch.scrollDirection = "next";
      this.transition = this.speed;
      this.slideX = this.$refs.sliderRef ? -this.$refs.sliderRef.offsetWidth:this.offsetWidth;
    },
    transitionEnd() {
      let cur = 0;
      if (this.touch.scrollDirection === false) {
        cur = this.currentIndex;
      } else {
        if (this.touch.scrollDirection === "prev") {
          if (this.currentIndex === 0) {
            cur = this.list.length - 1;
          } else {
            cur = this.currentIndex - 1;
          }
        } else {
          if (this.currentIndex !== this.list.length - 1) {
            cur = this.currentIndex + 1;
          }
        }
      }
      if (this.currentIndex !== cur) {
        this.$emit("change", cur);
      }
      this.transition = 0;
      this.slideX = 0;
      this.currentIndex = cur;
    },
    setTimer() {
      const vm = this;
      if (this.time && this.auto) {
        this.touch.timer = setInterval(
          vm.clockwise ? vm.go_next : vm.go_prev,
          vm.time
        );
      }
    },
  },
  watch:{
    list(val){
      if(val.length>0){
        if (this.auto) this.setTimer();
      }
    }
  }
};
</script>
<style  scoped>
.content {
  width: 400%;
  display: flex;
}
.content img {
  display: block;
  width: 100%;
}
.vue-slide {
  overflow: hidden;
  position: relative;
  border-bottom: 1px solid gray;
}
.vue-slide .content {
  display: flex;
  width: 300%;
}
.vue-slide .content > div {
  width: 33.33333%;
  position: relative;
}
.vue-slide .content > div:first-child {
  margin-left: -33.33333%;
}
.vue-slide .content img {
  display: block;
  width: 100%;
}
.vue-slide .spot {
  width: 100%;
  position: absolute;
  bottom: 12px;
  display: flex;
  justify-content: center;
}
.vue-slide .spot > div {
  width: 5px;
  height: 5px;
  border: 1px solid #fff;
  border-radius: 50%;
  margin: 0 4px;
  box-shadow: 0 0 3px rgba(0, 0, 0, 0.2);
  transition: width 0.2s linear;
}
.vue-slide .spot > div.cur {
  width: 12px;
  border-radius: 6px;
  background-color: #fff;
}
.slider-btn {
  position: absolute;
  top: 0;
  z-index: 999;
  height: 100%;
  width: 50px;
  border: none;
  background: transparent;
  outline: 0;
  cursor: pointer;
}
.slider-icon-left {
  transform: rotate(45deg);
}
.slider-icon-right {
  transform: rotate(225deg);
}
.slider-icon {
  display: inline-block;
  width: 15px;
  height: 15px;
  border-left: 2px solid rgba(255, 255, 255, 0.6);
  border-bottom: 2px solid rgba(255, 255, 255, 0.6);
  transition: border 0.2s;
}
.slider-btn-left {
  left: 0;
  /* background: linear-gradient(90deg, rgba(0, 0, 0, 0.1), rgba(0, 0, 0, 0)); */
}

.slider-btn-right {
  right: 0;
  /* background: linear-gradient(-90deg, rgba(0, 0, 0, 0.1), rgba(0, 0, 0, 0)); */
}
</style>
