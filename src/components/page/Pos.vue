<template>
  <div class="pos">
    <el-row>
      <el-col :span='7' class="pos-order" id="order-list">
        <el-tabs>
          <el-tab-pane label="点餐">
            <el-table :data="tableData" border style="width:100%">
              <el-table-column prop="goodsName" label="商品名称"></el-table-column>
              <el-table-column prop="count" label="数量" width="50"></el-table-column>
              <el-table-column prop="price" label="金额" width="70"></el-table-column>
              <el-table-column label="操作" width="100" fixed="right">
                <template slot-scope="scope">
                  <el-button type="text" size="small" @click="delSingleGoods(scope.row)">删除</el-button>
                  <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                </template>
              </el-table-column>
            </el-table>
            <div class=" totalDiv">
              <small>数量:</small>{{totalCount}} &nbsp;&nbsp;&nbsp;&nbsp; <small>金额:</small>{{totalMoney}}元
            </div>
            <div class="order-btn">
              <el-button type="warning">挂单</el-button>
              <el-button type="danger" @click="delAllGoods()">删除</el-button>
              <el-button type="success" @click="checkout()">结账</el-button>
            </div>
          </el-tab-pane>
          <el-tab-pane label="挂单">

          </el-tab-pane>
          <el-tab-pane label="外卖">

          </el-tab-pane>
        </el-tabs>

      </el-col>
      <el-col :span='17'>
        <div class="often-goods">
          <div class="title">常用商品</div>
          <div class="often-goods-list">
            <ul>
              <li v-for="(item, index) in oftenGoods" :key="item.id" @click="addOrderList(item)">
                <span>{{item.goodsName}}</span>
                <span class="o-price">￥{{item.price}}元</span>
              </li>
            </ul>
          </div>
        </div>
        <div class="goods-type">
          <el-tabs>
            <el-tab-pane label="汉堡">
              <div>
                <ul class='cookList'>
                  <li v-for="(goods, index) in type0Goods" :key="index" @click="addOrderList(goods)">
                    <span class="foodImg">
                      <img src="goods.goodsImg" width="100%" alt="商品图片" />
                    </span>
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">￥{{goods.price }}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="小食">
              <div>
                <ul class='cookList'>
                  <li v-for="(goods, index) in type1Goods" :key="index" @click="addOrderList(goods)">
                    <span class="foodImg">
                      <img :src="goods.goodsImg" width="100%"></span>
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">￥{{goods.price }}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="饮料">
              <div>
                <ul class='cookList'>
                  <li v-for="(goods, index) in type2Goods" :key="index" @click="addOrderList(goods)">
                    <span class="foodImg">
                      <img :src="goods.goodsImg" width="100%"></span>
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">￥{{goods.price }}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="套餐">
              <div>
                <ul class='cookList'>
                  <li v-for="(goods, index) in type3Goods" :key="index">
                    <span class="foodImg">
                      <img :src="goods.goodsImg" width="100%"></span>
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">￥{{goods.price }}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
          </el-tabs>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
  import axios from 'axios';
  export default {
    name: 'pos',
    data() {
      return {
        tableData: [],
        oftenGoods: [],
        type0Goods: [],
        type1Goods: [],
        type2Goods: [],
        type3Goods: [],
        totalMoney: 0,
        totalCount: 0,
      }
    },
    methods: {
      //添加商品
      addOrderList(goods) {
        this.totalMoney = 0;
        this.totalCount = 0;
        //判断是否已经存在订单列表中
        let isHave = false;
        for (let i = 0; i < this.tableData.length; i++) {
          if (this.tableData[i].goodsId === goods.goodsId) {
            isHave = true;
          }
        }
        //根据判断值写逻辑
        if (isHave) {
          //改变列表中商品的数量
          let arr = this.tableData.filter(item => item.goodsId === goods.goodsId);
          arr[0].count++;
        } else {
          let newGoods = {
            goodsId: goods.goodsId,
            goodsName: goods.goodsName,
            price: goods.price,
            count: 1
          };
          this.tableData.push(newGoods);
        }
        //计算汇总价格和数量
        this.getAllMoney();
      },
      //删除单个商品
      delSingleGoods(goods) {
        this.tableData = this.tableData.filter(item => item.goodsId != goods.goodsId);
        this.getAllMoney();
      },
      //汇总数量和金额
      getAllMoney() {
        this.totalMoney = 0;
        this.totalCount = 0;
        if (this.tableData) {
          //计算汇总价格和数量
          this.tableData.forEach(element => {
            this.totalCount += element.count;
            this.totalMoney += this.totalMoney + (element.count * element.price);
          });
        }
      },
      //全部删除
      delAllGoods(){
        this.tableData = [];
        this.totalMoney = 0;
        this.totalCount = 0;
      },
      //模拟结账
      checkout(){
        if(this.totalCount != 0){
          this.tableData = [];
          this.totalMoney = 0;
          this.totalCount = 0;
          this.$message({
            message:'结账成功!',
            type:'success'
          });
        }else{
           this.$message({
            message:'还未点餐!',
            type:'error'
          });
        }
      }
    },
    created() {
      axios.get('https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/oftenGoods')
        .then(res => {
          // console.log(res);
          this.oftenGoods = res.data;
        })
        .catch(err => {
          // console.log(err);
          alert('网络错误!')
        });
      axios.get('https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/typeGoods')
        .then(res => {
          // console.log(res);
          this.type0Goods = res.data[0];
          this.type1Goods = res.data[1];
          this.type2Goods = res.data[2];
          this.type3Goods = res.data[3];
        })
        .catch(err => {
          // console.log(err);
          alert('网络错误!')
        });

    },
    mounted() {
      var orderHeight = document.body.clientHeight;
      // console.log(orderHeight);
      document.querySelector('#order-list').style.height = orderHeight + 'px';
    }
  }

</script>

<style scoped>
  .pos {
    font-size: 20px;
  }

  .pos-order {

    background-color: #F9FAFC;
    border-right: 1px solid #C0CCDA;
  }

  .order-btn {
    margin-top: 10px;
    text-align: center;
  }

  .title {
    height: 20px;
    border-bottom: 1px solid #D3DCE6;
    background-color: #F9FAFC;
    padding: 10px;
  }

  .often-goods-list ul li {
    list-style: none;
    float: left;
    border: 1px solid #E5E9F2;
    padding: 10px;
    margin: 5px;
    background-color: #fff;
    cursor: pointer;
  }

  .goods-type {
    clear: both;
  }

  .o-price {
    color: #58B7FF;
  }

  .often-goods-list {
    border-bottom: 1px solid #C0CCDA;
    height: auto;
    overflow: hidden;
    padding-bottom: 10px;
    background-color: #F9FAFC;
  }

  .cookList li {
    list-style: none;
    width: 23%;
    border: 1px solid #E5E9F2;
    height: auot;
    overflow: hidden;
    background-color: #fff;
    padding: 2px;
    float: left;
    margin: 2px;
    cursor: pointer;
  }

  .cookList li span {

    display: block;
    float: left;
  }

  .foodImg {
    width: 40%;
  }

  .foodName {
    font-size: 18px;
    padding-left: 10px;
    color: brown;
  }

  .foodPrice {
    font-size: 16px;
    padding-left: 10px;
    padding-top: 10px;
  }

  .totalDiv {
    height: auot;
    overflow: hidden;
    text-align: right;
    font-size: 16px;
    background-color: #fff;
    border-bottom: 1px solid #E5E9F2;
    padding: 10px;
  }

</style>
