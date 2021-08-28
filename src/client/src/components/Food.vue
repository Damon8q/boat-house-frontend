<template>

  <!-- ======== @Region: #content ======== -->
  <div id="content" class="p-0">
    <div id="features" class="container py-4 py-lg-6">
      <hr class="hr-lg mt-0 mb-3 w-10 mx-auto hr-primary" />
      <h2 class="text-center text-uppercase font-weight-bold my-0">
        点菜 也是 享受
      </h2>
      <h5 class="text-center font-weight-light mt-2 mb-0 text-muted">
          Great food, great people!
      </h5>
     <hr class="mb-5 w-50 mx-auto" />
        <!-- Products cards -->
        <div class="card-deck-wrapper">
          <div class="card-deck">
            <!-- Product 1 -->
            <div class="card product-card">
              <!-- Image & price content -->
              <div class="pos-relative">
                <img class="card-img-top img-fluid" src="assets/img/product/boathouse01.png" alt="Card image cap">
              </div>
              <!-- Content -->
              <div class="card-body">
                <h5 class="card-title">
                  蜂蜜烤猪肉
                </h5>

              </div>
              <div class="card-footer">
                <a id="a"  name="vote" class="btn btn-primary text-light" value="a" v-on:click="likeProduct('a')"> ❤ 好吃</a>
              </div>
            </div>
            <!-- Product 2 -->
            <div class="card product-card">
              <!-- Image & price content -->
              <div class="pos-relative">
                <img class="card-img-top img-fluid" src="assets/img/product/boathouse02.png" alt="Card image cap">
              </div>
              <!-- Content -->
              <div class="card-body">
                <h5 class="card-title">
                  牛肉haggis
                </h5>
              </div>
              <div class="card-footer">
                <a id="b"  name="vote" class="btn btn-primary text-light" value="b" v-on:click="likeProduct('b')"> ❤ 好吃</a>
              </div>
            </div>
            <!-- Product 3 -->
            <div class="card product-card">
              <!-- Image & price content -->
              <div class="pos-relative">
                <img class="card-img-top img-fluid" src="assets/img/product/boathouse03.png" alt="Card image cap">
              </div>
              <!-- Content -->
              <div class="card-body">
                <h5 class="card-title">
                  红酒炖羊腿
                </h5>
              </div>
              <div class="card-footer">
                <a id="c"  name="vote" class="btn btn-primary text-light" value="c" v-on:click="likeProduct('c')"> ❤ 好吃</a>
              </div>
            </div>

          </div>
          <br/>

          </div>

        </div>

      <div class="card-deck-wrapper"  >
        <div class="card-deck"> 
          <div class="card product-card"  v-for="item in foodList" :key="item.Id">
                <!-- Image & price content -->
                  <div class="pos-relative">
                    <img class="card-img-top img-fluid"  src="assets/img/product/boathouse06.png" alt="Card image cap">
                  </div>
                <!-- Content -->
                <div class="card-body">
                  <h5 class="card-title">
                    {{item.Name}}
                  </h5>
                </div>
                <div class="card-footer">
                  <div class="row">
                  <div class="col-md-4">{{item.Price}}元</div>
                  <div class="col-md-4 col-md-offset-6">
                      <a id="a"  name="vote" class="btn btn-primary text-light"  @click="AddFoodToCart(item)">加入购物车</a>
                  </div>
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
  name: 'Food',
  data () {
    return {
      foodList: [],
      foodCategory: []
    }
  },
  mounted: function () {
    let _this = this
    this.axios.get('api/foodcategories').then(function (result) {
      if (result.status === 200) {
        _this.foodCategory = result.data
        _this.GetFoodList()
      }
    })
  },
  methods: {
    GetFoodList: function () {  
     let _this = this
      this.axios.get('api/foods').then(function (result) {
        if (result.status === 200) {
          _this.foodList = result.data.data
          _this.foodList.map(item => {
            let categoryItem = _this.foodCategory.find(i => i.Id === item.CategoryId)
            item.Category = categoryItem.Name
          })
          console.log(_this.foodList)
        }
      })
    }, 
    AddFoodToCart: function (e) {
      let _this = this 
      const userId = _this.$parent.getCookie("userId");   
      if(userId === '' || userId === undefined){
        $("#login-modal").modal('show');
        return;
      }
      let postData = {
            userid: userId,
            foodid: parseInt(e.Id),
            num: 1,
            comment: ""
         };

       // http://127.0.0.1:8081/api/v1.0/BoatHouse/ShopCart 
      const url = '/api/food/shopcart'; 
      this.axios.post(url, postData).then(function (result) {
        console.log(result)
        if (result.status === 200) {
          console.log("添加购物车成功"); 
          _this.$parent.GetShopCartInfo(); 
        }
      },
       function(res){          
          console.log("添加购物车失败",res);
       })
    }

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.food-card {
  text-align: center;
  font-size: 12px;
}
ul li {
  list-style-type: none;
}
.name-span {
  font-size: 10px;
  color: gray;
}
</style>
