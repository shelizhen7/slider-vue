<template>
  <li class="tabs-item" :class="{'active':isActive}">
    <div class="tabs-link" :class="{'remove-border':isLastChild}" style="border-right: 1px solid #dbdbdb" @click="onActive">
      <slot/>
    </div>
  </li>
</template>
<script>
export default {
  name: "tab-panel",
  props: {
    value: {
      type: String,
    },
    name:{
      type: String,
    },
    activeName:{
      type: String
    }
  },
  data(){
    return {
      isLastChild:false,
      isActive:false
    }
  },
  methods:{
    handleLastChild(){
      this.isLastChild = true
    },
    handleActive(val){
      this.isActive = val === this.name
    },
    onActive(){
      this.$parent.$emit('handleClick',this.name)
    }
  }
};
</script>
<style scoped>
.tabs-item {
  display: block;
  list-style: none;
  padding: 0;
  flex-grow: 1;
  color: #dbdbdb;
  padding: 10px;
  text-align: center;
  border-bottom: 3px solid transparent;
  transition: border 1s ease
}
.tabs-item.active {
  border-color: #f58024;
  color:#fff;
}
.remove-border{
  border-color:transparent !important;
}
</style>