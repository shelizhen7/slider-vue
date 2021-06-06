<template>
  <div>
    <Slider :list="list"  :auto="true" @change="handleChange"/>
    <p style="margin-top:10px">{{currentIndex}}</p>
  </div>
</template>

<script>
import axios from "axios";
import Slider from "./components/Slider.vue";
export default {
  name: "App",
  components: {
    Slider,
  },
  data() {
    return {
      list: [],
      sliderValue: 2,
      currentIndex:1
    };
  },
  methods: {
    changeIndex(index) {
      this.sliderValue = index;
    },
    getImageList() {
      const vm = this;
      axios.get("https://jsonplaceholder.typicode.com/photos").then((res) => {
        if (res.status == 200) {
          // vm.list = []
          vm.list = res.data.filter((e) => e.id <= 6).map((e) => e.url);
        }
      });
    },
    handleChange(cur){
      this.currentIndex = cur+1
    }
  },
  created() {
    this.getImageList();
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

p {
  margin: 0;
}
</style>
