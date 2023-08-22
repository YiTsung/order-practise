<template>
  <div class="home">
    <img src="../assets/Q-cup1.png" alt="" class="Q-cup1">
    <img src="../assets/Q-cup2.png" alt="" class="Q-cup2">
    <img src="../assets/Q-cup3.png" alt="" class="Q-cup3">
    <img src="../assets/Q-cup4.png" alt="" class="Q-cup4">
    <div class="home">
      <h1 class="title">好好喝飲品</h1>
      <div class="container gx-2">
        <div class="row gx-3 bg-light py-3">
          <div class="col-md-4">
            <div class="list-group">
              <a href="#" class="list-group-item list-group-item-action" v-for="item in products" @click.prevent="selceProduct(item)"
              :class="{'active': itemSelected.name === item.name}" :key="item.name">
                <h6 class="card-title mb-1">{{item.name}}</h6>
                <div class="d-flex align-items-center justify-content-between">
                  <p class="mb-0"><small>{{item.engName}}</small></p>
                  <p class="mb-0"><small>{{item.price}}</small></p>
                </div><!--被選擇項目加深顏色 active-->
              </a>
            </div>
          </div>
          <div class="col-md-8">
            <div class="card mb-2">
              <div v-if="!itemSelected.name" class="position-absolute text-white d-flex align-items-center justify-content-center active" style="top: 0;bottom: 0;left: 0;right: 0;background-color: rgba(0, 0, 0, 0.65); z-index: 100;">請先選擇飲品</div>
              <div class="card-body px-4">
                <div class="mb-3">
                  <label for="productNum" class="form-label">數量</label>
                  <input type="number" class="form-control" id="productNum" placeholder="數量" min="1" v-model="itemSelected.count">
                </div>
                <div class="mb-3">
                  <label for="productNum" class="form-label d-block">冰塊*</label>
                  <div class="form-check form-check-inline" v-for="(ice,key) in iceType" :key="ice + key">
                    <input class="form-check-input" name="iceType" type="radio" :id="ice + key"
                    :value="ice" v-model="itemSelected.ice" >
                    <label class="form-check-label" :for="ice + key">{{ice}}</label>
                  </div>
                </div>
                <div class="mb-3">
                  <label for="productNum" class="form-label d-block">甜度*</label>
                  <div class="form-check form-check-inline"
                  v-for="(sugar,key) in sugarType" :key="sugar + key">
                    <input class="form-check-input" name="sugarType" type="radio" :id="sugar + key"
                    :value="sugar" v-model="itemSelected.sugar">
                    <label class="form-check-label" :for="sugar + key">{{sugar}}</label>
                  </div>
                </div>
                <div class="mb-3">
                  <label for="productNum" class="form-label d-block">加料</label>
                  <div class="form-check form-check-inline" v-for="(topping,key) in toppingsType" :key="topping + key">
                    <input class="form-check-input" type="checkbox"
                    :id="topping + key" :value="topping" v-model="itemSelected.toppings">
                    <label class="form-check-label"
                    :for="topping + key">{{topping}}</label>
                  </div>
                </div>
                <div class="mb-3">
                  <label for="productNotice" class="form-label" >備註</label>
                  <textarea class="form-control" id="productNotice" rows="2" v-model="itemSelected.remark"></textarea>
                </div>
                <div class="d-flex gap-2">
                  <button class="btn btn-outline-primary w-100" type="button" @click="reset">取消</button>
                  <button class="btn btn-primary w-100" type="button" @click="addToOrder(itemSelected)">加入</button>
                </div>
              </div>
            </div>
            <div class="card" v-show="orderList.length !== 0 ">
              <div class="card-body">
                <table class="table">
                  <thead>
                    <tr>
                      <th scope="col">品項</th>
                      <th scope="col">冰塊</th>
                      <th scope="col">甜度</th>
                      <th scope="col">加料</th>
                      <th scope="col">單價</th>
                      <th scope="col">數量</th>
                      <th scope="col">小計</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr  v-for="(item, key) in orderList" :key="item+key">
                      <th scope="row">{{item.name}}<br>
                        <small class="text-muted fw-normal">{{item.remark}}</small>
                      </th>
                      <td>{{ item.ice }}</td>
                      <td>{{item.sugar}}</td>
                      <td>{{item.toppings.toString()}}</td>
                      <td>{{item.price + item.toppings.length*10}}</td>
                      <td>{{item.count}}</td>
                      <td class="text-end">{{item.total}}</td>
                    </tr>
                  </tbody>
                </table>
                <p class="text-end">共 NT$ {{orderTotal}} 元</p>
                <button class="btn btn-sm btn-secondary w-100" @click="deleteOrderList">重新選擇</button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  data () {
    return {
      itemSelected: {

      },
      orderList: [],
      orderTotal: 0,
      iceType: ['正常冰', '少冰', '微冰', '去冰', '熱'],
      sugarType: ['全糖', '七分', '半糖', '三分', '無糖'],
      toppingsType: ['珍珠', '粉條', '小粉圓', '椰果', '芋頭'],
      products: [
        {
          name: '珍珠鮮奶茶',
          engName: 'Pearl Milk Tea',
          price: 60
        },
        {
          name: '鮮奶茶',
          engName: 'Milk Tea',
          price: 50
        },
        {
          name: '古意冬瓜茶',
          engName: 'Winter Melon Drink',
          price: 30
        },
        {
          name: '蜜香紅茶',
          engName: 'Black Tea',
          price: 30
        },
        {
          name: '包種青茶',
          engName: 'Black Tea',
          price: 35
        },
        {
          name: '檸檬烏龍',
          engName: 'Lemon Oolong Tea',
          price: 55
        },
        {
          name: '薑母茶',
          engName: 'Ginger Tea',
          price: 55
        },
        {
          name: '青草茶',
          engName: 'Herbal Tea',
          price: 35
        },
        {
          name: '金桔檸檬',
          engName: 'Kumquat Lemonade',
          price: 40
        },
        {
          name: '柳澄青茶',
          engName: 'Orange Mountain Tea',
          price: 45
        }
      ]
    }
  },
  methods: {
    selceProduct (product) {
      this.itemSelected = {
        ...product,
        toppings: [],
        sugar: [],
        ice: [],
        remark: '',
        count: 1
      }
    },
    countTotal () {
      this.orderTotal = 0
      this.orderList.forEach(item => {
        this.orderTotal += item.total
      })
    },
    reset () {
      this.itemSelected = {}
    },
    addToOrder (product) {
      // eslint-disable-next-line eqeqeq
      if (product.ice == false) {
        product.ice = '正常冰'
      }
      // eslint-disable-next-line eqeqeq
      if (product.sugar == false) {
        product.sugar = '全糖'
      }
      const order = {
        ...product,
        total: (product.price + product.toppings.length * 10) * product.count
      }
      this.orderList.push(order)
      this.countTotal()
      this.reset()
    },
    deleteOrderList () {
      this.orderList = []
      this.orderTotal = 0
    }
  }
}
</script>
<style>
body{
    margin-top: 50px;
    margin-bottom: 50px;
    font-family: var(--bs-font-sans-serif);
    font-size: 1rem;
    font-weight: 400;
    line-height: 1.5;
    color: #212529;
    background-color: #ffce7f;
    position: relative;
    -webkit-text-size-adjust: 100%;
    -webkit-tap-highlight-color: rgba(0, 0, 0, 0); }
  [tabindex="-1"]:focus:not(:focus-visible) {
    outline: 0 !important
}
.title{
  text-align: center;
  font-weight: bold;
  color: brown;
}
img{
    width: 150px;
    height: 150px;
    z-index: -1;
    position: absolute;
}
.Q-cup1{
    top: 0px;
    left: 0px;
}
.Q-cup2{
    top: 0px;
    right: 0px;
}
.Q-cup3{
    bottom: 0px;
    left: 0px;
}
.Q-cup4{
    bottom: 0px;
    right: 0px;
}
</style>
