<template>
  <div class="left-nav">
    <ul>
      <li>
        <i class="icon iconfont icon-31goumaichongzhi"></i>
        <div>收银</div>
      </li>
      <li>
        <i class="icon iconfont icon-dianpu"></i>
        <div>店铺</div>
      </li>
      <li>
        <i class="icon iconfont icon-hanbao"></i>
        <div>商品</div>
      </li>
      <li>
        <i class="icon iconfont icon-huiyuanqia"></i>
        <div>会员</div>
      </li>
      <li>
        <i class="icon iconfont icon-zuanshi"></i>
        <div>统计</div>
      </li>
      <li>
        <i class="icon iconfont icon-shezhi"></i>
        <div>设置</div>
      </li>
    </ul>
  </div>
</template>

<script>
  export default {
    data() {
      return {}
    },
    methods: {},
    components: {}
  }

</script>

<style>
  .left-nav {
    float: left;
    width: 5%;
    min-width: 60px;
    height: 100%;
    background-color: #1D8CE0;
    color: #fff;
    font-size: 10px;
  }

  .iconfont {
    font-size: 25px;
  }

  .left-nav ul {
    padding: 0;
    margin: 0;
  }

  .left-nav ul>li {
    list-style: none;
    text-align: center;
    border-bottom: 1px solid #20a0ff;
    padding: 10px;
  }

</style>
