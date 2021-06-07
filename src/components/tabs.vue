<template>
  <div class="tab-container">
    <ul class="tabs-box">
      <!-- <li class="tabs-item active">
        <div class="tabs-link" style="border-right: 1px solid #dbdbdb">
          Avaliable Items
        </div>
      </li>
      <li class="tabs-item" @click="tabs()">
        <div class="tabs-link">Information One</div>
      </li> -->
      <slot />
    </ul>
  </div>
</template>
<script>
export default {
  name: "tabs",
  props: {
    value: {
      type: String,
    },
  },
  mounted() {
    const len = this.$children.length;
    this.$children[len - 1].handleLastChild();
    const vm = this;
    this.$children.map((e) => {
      e.handleActive(vm.value);
    });
    this.$on('handleClick',(val)=>{
      this.$children.map((e) => {
        e.handleActive(val);
      });
    })
  },
  methods: {
    handleClick(val) {
      this.$children.map((e) => {
        e.handleActive(val);
      });
    }
  },
};
</script>
<style scoped>
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
.tab-container {
  background-color: #585858;
}
</style>