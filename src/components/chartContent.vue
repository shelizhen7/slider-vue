<template>
  <div class="content">
    <div class="chart-detail">
      <h4>Product Item Summary</h4>
      <div class="deposite"><span class="title">Deposited</span><span class="num">$2500.00</span></div>
      <div class="withdrawn"><span class="title">Withdrawn</span><span class="num">$700.00</span></div>
    </div>
    <div class="pie">
      <div class="colorWheel">
      <div class="piece"></div>
  </div>
    </div>
  </div>
</template>
<script>
</script>
<style scoped>
.content{
  display: flex;
  height:100%;
}
.chart-detail{
  width:40%;
  padding:20px;
  color:#dbdbdb
}
.pie{
  flex-grow:1;
}
.deposite,.withdrawn{
  margin:20px 0;
}
.deposite span,.withdrawn span{
  display: block;
}
.deposite .title{
  color:#F58024;
  font-weight:600;
}
.withdrawn .title{
  color:#E51C1C;
  font-weight:600;
}
.deposite .num,.withdrawn .num{
  font-size:0.8rem;
  margin-top:.4rem;
}
.colorWheel {
  position: relative;
  width: 8em;
  height: 8em;
  background: linear-gradient(90deg, #E51C1C 99.9%, transparent 99.9%),linear-gradient(-90deg, #F58024 99.9%, transparent 99.9%) 100% 0, linear-gradient(-90deg, transparent 0.01%, #E51C1C 0.01%) 0 100%,linear-gradient(90deg, transparent 0.01%, #E51C1C 0.01%) 100% 100%;
  background-repeat: no-repeat;
  background-size: 50% 50%;
  border:1px solid #ccc;
  border-radius: 50%;
  margin: 1.875rem auto;
}
.piece{
  position: absolute;
  left: 8%;
  top: -2.6%;
  width: 100%;
  height: 100%;
  background-repeat: no-repeat;
  background-size: 50% 50%;
  border-radius: 50%;
}
</style>
