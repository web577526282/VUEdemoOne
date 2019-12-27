<template>
  <div class="pos">
    <div>
      <el-row>
        <el-col id="order-list" :span='7'>
          <el-tabs stretch>
            <el-tab-pane label="点餐">
              <el-table :data="tableData" border style="width: 100%">
                <el-table-column align=center prop="goodsName" label="商品"></el-table-column>
                <el-table-column align=center prop="count" label="数量" width="50"></el-table-column>
                <el-table-column align=center prop="price" label="金额" width="70"></el-table-column>
                <el-table-column align=center label="操作" width="100" fixed="right">
                  <template slot-scope="scope">
                    <el-button type="text" size="small" @click="delSingleGoods(scope.row)">删除</el-button>
                    <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                  </template>
                </el-table-column>
              </el-table>
              <div class="totalDiv">
                <small>数量：</small>{{totalCount}} &nbsp;&nbsp;&nbsp;&nbsp; <small>金额：</small>{{totalMoney}}元
              </div>
              <div class="div-btn">
                <!-- <el-button type="warning">挂单</el-button> -->
                <el-button type="danger" @click="delAllGoods">删除</el-button>
                <el-button type="success">结账</el-button>
              </div>
            </el-tab-pane>
            <el-tab-pane label="挂单">
              挂单
            </el-tab-pane>
            <el-tab-pane label="外卖">
              外卖
            </el-tab-pane>
          </el-tabs>
        </el-col>
        <!--商品展示-->
        <el-col id="order-list" :span="17">
          <div class="often-goods">
            <div class="title">常用商品</div>
            <div class="often-goods-list">
              <ul>
                <li v-for="(item,index) in oftenGoods" :key="item.goodsId" @click="addOrderList(item)">
                  <span>{{item.goodsName}}</span>
                  <span class="o-price">￥{{item.price}}元</span>
                </li>
              </ul>
            </div>
          </div>
          <div class="goods-type">
            <el-tabs>
              <el-tab-pane label="汉堡">
                <ul class='cookList'>
                  <li v-for="(add,index) in type0Goods" :key="add.goodsId" @click="addOrderList(add)">
                    <span class="foodImg"><img :src="add.goodsImg" width="110%"></span>
                    <span class="foodName">{{add.goodsName}}</span>
                    <span class="foodPrice">￥{{add.price}}元</span>
                  </li>
                </ul>
              </el-tab-pane>
              <el-tab-pane label="小食">
                小食
              </el-tab-pane>
              <el-tab-pane label="饮料">
                饮料
              </el-tab-pane>
              <el-tab-pane label="套餐">
                套餐
              </el-tab-pane>
            </el-tabs>
          </div>
        </el-col>
      </el-row>
    </div>
  </div>
</template>
<script>
  import axios from 'axios'
  export default {
    name: 'Pos',
    data() {
      return {
        tableData: [],
        oftenGoods: [
          {
            goodsId: 1,
            goodsName: '香辣鸡腿堡',
            price: 18,
            count: 1,
          }, {
            goodsId: 2,
            goodsName: '田园鸡腿堡',
            count: 1,
            price: 15
          }, {
            goodsId: 3,
            goodsName: '和风汉堡',
            count: 1,
            price: 15
          }, {
            goodsId: 4,
            goodsName: '快乐全家桶',
            count: 1,
            price: 80
          }, {
            goodsId: 5,
            goodsName: '脆皮炸鸡腿',
            price: 10,
            count: 1
          }, {
            goodsId: 6,
            goodsName: '魔法鸡块',
            count: 1,
            price: 20
          }, {
            goodsId: 7,
            goodsName: '可乐大杯',
            count: 1,
            price: 10
          }, {
            goodsId: 8,
            goodsName: '雪顶咖啡',
            count: 1,
            price: 18
          }, {
            goodsId: 9,
            goodsName: '大块鸡米花',
            count: 1,
            price: 15
          }, {
            goodsId: 20,
            goodsName: '香脆鸡柳',
            count: 1,
            price: 17
          }

        ],
        type0Goods: [
          {
            goodsId: 1,
            goodsImg: "https://ss0.bdstatic.com/94oJfD_bAAcT8t7mm9GUKT-xh_/timg?image&quality=100&size=b4000_4000&sec=1577268268&di=0a31436738b6b7ee0104fddc39fb1b9a&src=http://y1.ifengimg.com/67563ef4e5c0caab/2014/0310/rdn_531d79d3bbc33.jpg",
            goodsName: '香辣鸡腿堡',
            price: 18
          }, {
            goodsId: 2,
            goodsImg: "https://ss0.bdstatic.com/94oJfD_bAAcT8t7mm9GUKT-xh_/timg?image&quality=100&size=b4000_4000&sec=1577268268&di=0a31436738b6b7ee0104fddc39fb1b9a&src=http://y1.ifengimg.com/67563ef4e5c0caab/2014/0310/rdn_531d79d3bbc33.jpg",
            goodsName: '田园鸡腿堡',
            price: 15
          }, {
            goodsId: 3,
            goodsImg: "https://ss0.bdstatic.com/94oJfD_bAAcT8t7mm9GUKT-xh_/timg?image&quality=100&size=b4000_4000&sec=1577268268&di=0a31436738b6b7ee0104fddc39fb1b9a&src=http://y1.ifengimg.com/67563ef4e5c0caab/2014/0310/rdn_531d79d3bbc33.jpg",
            goodsName: '和风汉堡',
            price: 15
          }, {
            goodsId: 4,
            goodsImg: "https://ss0.bdstatic.com/94oJfD_bAAcT8t7mm9GUKT-xh_/timg?image&quality=100&size=b4000_4000&sec=1577268268&di=0a31436738b6b7ee0104fddc39fb1b9a&src=http://y1.ifengimg.com/67563ef4e5c0caab/2014/0310/rdn_531d79d3bbc33.jpg",
            goodsName: '快乐全家桶',
            price: 80
          }, {
            goodsId: 5,
            goodsImg: "https://ss0.bdstatic.com/94oJfD_bAAcT8t7mm9GUKT-xh_/timg?image&quality=100&size=b4000_4000&sec=1577268268&di=0a31436738b6b7ee0104fddc39fb1b9a&src=http://y1.ifengimg.com/67563ef4e5c0caab/2014/0310/rdn_531d79d3bbc33.jpg",
            goodsName: '脆皮炸鸡腿',
            price: 10
          }, {
            goodsId: 6,
            goodsImg: "https://ss0.bdstatic.com/94oJfD_bAAcT8t7mm9GUKT-xh_/timg?image&quality=100&size=b4000_4000&sec=1577268268&di=0a31436738b6b7ee0104fddc39fb1b9a&src=http://y1.ifengimg.com/67563ef4e5c0caab/2014/0310/rdn_531d79d3bbc33.jpg",
            goodsName: '魔法鸡块',
            price: 20
          }, {
            goodsId: 7,
            goodsImg: "https://ss0.bdstatic.com/94oJfD_bAAcT8t7mm9GUKT-xh_/timg?image&quality=100&size=b4000_4000&sec=1577268268&di=0a31436738b6b7ee0104fddc39fb1b9a&src=http://y1.ifengimg.com/67563ef4e5c0caab/2014/0310/rdn_531d79d3bbc33.jpg",
            goodsName: '可乐大杯',
            price: 10
          }, {
            goodsId: 8,
            goodsImg: "https://ss0.bdstatic.com/94oJfD_bAAcT8t7mm9GUKT-xh_/timg?image&quality=100&size=b4000_4000&sec=1577268268&di=0a31436738b6b7ee0104fddc39fb1b9a&src=http://y1.ifengimg.com/67563ef4e5c0caab/2014/0310/rdn_531d79d3bbc33.jpg",
            goodsName: '雪顶咖啡',
            price: 18
          }, {
            goodsId: 9,
            goodsImg: "https://ss0.bdstatic.com/94oJfD_bAAcT8t7mm9GUKT-xh_/timg?image&quality=100&size=b4000_4000&sec=1577268268&di=0a31436738b6b7ee0104fddc39fb1b9a&src=http://y1.ifengimg.com/67563ef4e5c0caab/2014/0310/rdn_531d79d3bbc33.jpg",
            goodsName: '大块鸡米花',
            price: 15
          }, {
            goodsId: 20,
            goodsImg: "https://ss0.bdstatic.com/94oJfD_bAAcT8t7mm9GUKT-xh_/timg?image&quality=100&size=b4000_4000&sec=1577268268&di=0a31436738b6b7ee0104fddc39fb1b9a&src=http://y1.ifengimg.com/67563ef4e5c0caab/2014/0310/rdn_531d79d3bbc33.jpg",
            goodsName: '香脆鸡柳',
            price: 17
          }

        ],
        totalMoney: 0,
        totalCount: 0
      }
    },
    mounted() {
      var orderHeight = document.body.clientHeight;
      document.getElementById("order-list").style.height = orderHeight + 'px';
    },
    created() {
      // axios.get('http://old.jspag.com/DemoApi/oftenGoods.php')
      // .then(response=>{
      //   console.log(response);
      //   // this.oftenGoods=
      // })
      // .catch(error=>{
      //   console.log(error);
      // })
    },
    methods: {
      addOrderList(goods) {
        // console.log(goods)
        this.totalCount = 0;
        this.totalMoney = 0;
        let isHave = false;
        for (let i = 0; i < this.tableData.length; i++) {
          if (this.tableData[i].goodsId == goods.goodsId) {
            isHave = true;
          }
        }
        if (isHave) {
          let arr = this.tableData.filter(o => o.goodsId == goods.goodsId);
          arr[0].count++;
        } else {
          let newGoods = { goodsId: goods.goodsId, goodsName: goods.goodsName, price: goods.price, count: 1 };
          this.tableData.push(newGoods)
        }
        this.tableData.forEach((element) => {
          this.totalCount += element.count;
          this.totalMoney = this.totalMoney + (element.price * element.count)
        })
      },
      getAllMoney() {
        this.totalCount = 0;
        this.totalMoney = 0;
        if (this.tableData) {
          this.tableData.forEach((element) => {
            this.totalCount += element.count;
            this.totalMoney = this.totalMoney + (element.price * element.count);
          });
        }
      },
      delSingleGoods(goods) {
        this.tableData = this.tableData.filter(add => add.goodsId != goods.goodsId);
    //     this.tableData = this.tableData.filter((add) =>{
    //      return add.goodsId != goods.goodsId
    // } )
        this.getAllMoney()
      },
      //汇总数量和金额
     
      delAllGoods() {
        this.tableData = [];
        this.totalCount = 0;
        this.totalMoney = 0;
      },
    },
  }
</script>
<style scoped>
  .title {
    height: 18.5px;
    border-bottom: 1px solid #D3DCE6;
    background-color: #F9FAFC;
    padding: 10px;
    /* box-sizing: border-box; */
  }

  .often-goods-list ul li {
    list-style: none;
    float: left;
    border: 1px solid #E5E9F2;
    padding: 10px;
    margin: 5px;
    background-color: #fff;
  }

  .o-price {
    color: #58B7FF;
  }

  .div-btn {
    text-align: center;
    margin-top: 15px;
  }

  .goods-type {
    clear: both;
    padding-left: 15px;
    text-align: center;
    box-sizing: border-box;
  }

  .cookList li {
    list-style: none;
    width: 30%;
    border: 1px solid #E5E9F2;
    height: auot;
    overflow: hidden;
    background-color: #fff;
    padding: 2px;
    float: left;
    margin: 4px;
    margin-bottom: 8px;
  }

  .cookList li span {

    display: block;
    float: left;
  }

  .foodImg {
    width: 40%;
  }

  .foodName {
    font-size: 16px;
    margin-left: 8%;
    color: brown;
    width: 50%;
  }

  .foodPrice {
    font-size: 16px;
    margin-left: 10%;
    padding-top: 15px;
    width: 50%;
  }

  .totalDiv {
    background-color: #fff;
    padding: 10px;
    border-bottom: 1px solid #D3dce6;
    text-align: center;
  }
</style>