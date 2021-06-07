<template>
  <div>
    <div id="app" v-show="!isLoading">
      <div class="container">
        <vue-header />
        <chart-content></chart-content>
      </div>
      <p style="color: #dbdbdb; padding: 30px 40px">
        Lorem ipsum dummy text.Lorem isum not just dummy text
      </p>
      <div class="tab-container">
        <ul class="tabs-box">
          <li class="tabs-item active">
            <div class="tabs-link" style="border-right: 1px solid #dbdbdb;">Avaliable Items</div>
          </li>
          <li class="tabs-item">
            <div class="tabs-link">Information One</div>
          </li>
        </ul>
      </div>
      <Slider :list="list" />
    </div>
    <div v-if="isLoading" class="loading">Loading...</div>
  </div>
</template>

<script>
import axios from "axios";
import vueHeader from "./components/header.vue";
import chartContent from "./components/chartContent.vue";
import Slider from "./components/Slider.vue";

export default {
  name: "App",
  components: {
    vueHeader,
    Slider,
    chartContent,
  },
  data() {
    return {
      list: [],
      sliderValue: 2,
      currentIndex: 1,
      isLoading: true,
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
          vm.list = res.data.filter((e) => e.id <= 6).map((e) => e.thumbnailUrl);
        }
        this.isLoading = false;
      });
    },
    handleChange(cur) {
      this.currentIndex = cur + 1;
    },
  },
  created() {
    this.getImageList();
  },
  watch:{
    isLoading(val){
      if(val == false){
        document.body.style.backgroundColor = '#505050'
      }
    }
  }
};
</script>

<style>
body {
  margin: 0;
  background-color: #000;
}
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  height: 100%;
  /* border-top-left-radius:3px;
  border-top-right-radius:3px */
  border-radius: 3px;
}
.loading{
  position: absolute;
  top: 46%;
  left: 40%;
  color:#dbdbdb;
}
.container {
  height: 15rem;
  background-color: #000;
  padding: 30px 20px;
  border-radius: 10px;
}
p {
  margin: 0;
}
.tabs-box {
  -webkit-box-flex: 1;
  flex: 1;
  display: flex;
  flex-direction: row;
  margin: 0;
  padding: 0;
  -webkit-box-orient: horizontal;
  -webkit-box-direction: normal;
  /* -webkit-box-pack: justify;
  justify-content: space-between; */
}
.tabs-item {
  display: block;
  list-style: none;
  padding: 0;
  flex-grow: 1;
  color: #dbdbdb;
  padding: 10px;
  text-align: center;
  /* border-bottom: 3px solid transparent; */
  border-bottom: 3px solid transparent;
}
.tabs-item.active {
  border-color: #f58024;
}
.tab-container {
  background-color: #585858;
}
.tabs-link {
}
</style>

