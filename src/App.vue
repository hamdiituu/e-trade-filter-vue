<template>
  <div class="container">
    <div class="content">
      <h1 class="header-title">Shop</h1>
      <div class="order-product">
        <select>
          <option selected disabled value="" >Sıralama Seçiniz</option>
          <option>Fiyata Göre Artan</option>
          <option>Fiyata Göre Azalan</option>
        </select>
      </div>
      <div class="product-list">
        <product-card :list="product" v-for="product in dataList.products" :key="product.id" />
      </div>
    </div>
    <div class="filter-bar sticky">
      <div class="filter">
        <!-- Trades Filter Box-->

        <div class="filter-data">
          <p class="title">Marka</p>
          <div class="filter-check">
            <div v-for="trade in dataList.trade" :key="trade.id">
              <input
                v-model="tradeFilters"
                :value="trade.id"
                :id="trade.name"
                type="checkbox"
                v-bind:name="trade.name"
              />
              <label v-bind:for="trade.name">{{trade.name}}</label>
            </div>
          </div>
        </div>

        <!-- Trades Filter Box-->

        <!-- Genders Filter Box-->

        <div class="filter-data">
          <p class="title">Cinsiyet</p>
          <div class="filter-check">
            <div v-for="gender in dataList.genders" :key="gender.id">
              <input
                v-model="genderFilters"
                :value="gender.id"
                type="checkbox"
                v-bind:name="gender.name"
              />
              <label v-bind:for="gender.name">{{gender.name}}</label>
            </div>
          </div>
        </div>

        <!-- Genders Filter Box-->

        <!-- Colors Filter Box-->

        <div class="filter-data">
          <p class="title">Renk</p>
          <div class="filter-check">
            <div v-for="color in dataList.colors" v-bind:key="color.id">
              <input
                v-model="colorFilters"
                :value="color.id"
                type="checkbox"
                v-bind:name="color.name"
              />
              <label v-bind:for="color.name">{{color.name}}</label>
            </div>
          </div>
        </div>

        <!-- Sizes Filter Box-->

        <div class="filter-data">
          <p class="title">Beden</p>
          <div class="filter-check">
            <div v-for="number in dataList.numbers" :key="number.id">
              <input
                v-model="sizeFilters"
                :value="number.name"
                type="checkbox"
                id="scales"
                :name="number.name"
              />
              <label :for="number.name">{{number.name}}</label>
            </div>
          </div>
        </div>

        <!-- Sizes Filter Box-->
      </div>
    </div>
  </div>
</template>

<script>
import Card from "./components/Card.vue";
export default {
  name: "App",
  data() {
    return {
      dataList: [],
      tradeFilters: [],
      genderFilters: [],
      colorFilters: [],
      sizeFilters: []
    };
  },
  components: {
    "product-card": Card
  },
  methods: {
    fetchDatas: function() {
      fetch("https://e-trade-api.firebaseio.com/.json")
        .then(res => res.json())
        .then(res => this.joinDatas(res))
        //.then(res=>console.log(res))
        .catch(err => console.log(err));
    },
    joinDatas: function(data) {
      // console.log(data);
      let products = data.products;
      products.map(p => {
        p.visible = true;
        p.color = data.colors.find(c => c.id === p.colorId).name;
        p.genderName = data.genders.find(c => c.id === p.gender).name;
        p.trade = data.trade.find(t => t.id === p.tradeId).name;
      });
      data.products = products;
      this.dataList = data;
      // console.log(data["colors"])
    },
    filterByData: function() {
      let result = this.dataList;

      result.products.forEach(p => {
        if (this.tradeFilters.length > 0) {
          !this.tradeFilters.find(t => t === p.tradeId)
            ? (p.visible = false)
            : (p.visible = true);
        } else {
          p.visible = true;
        }
      });

      // console.log(result.products)

      result.products.forEach(p => {
        if (this.colorFilters.length > 0) {
          !this.colorFilters.find(c => c === p.colorId) && (p.visible = false);
        }
      });

      result.products.forEach(p => {
        if (this.genderFilters.length > 0) {
          !this.genderFilters.find(g => g === p.gender) && (p.visible = false);
        }
      });

      result.products.forEach(p => {
        if (this.sizeFilters.length > 0) {
          let visible = false;
          this.sizeFilters.forEach(s => {
            p.number.find(n => n === s) && (visible = true);
          });
          !visible && (p.visible = false);
        }
      });

      this.dataList = result;
    }
  },
  computed: {},
  created() {
    this.fetchDatas();
  },
  updated() {
    this.filterByData();
  }
};
</script>

<style>
*,
html,
body {
  margin: 0;
  padding: 0;
  font-family: "Lucida Console", Monaco, monospace;
}
.container {
  display: flex;
  flex-direction: row;
}
.sticky {
  position: sticky;
  top: 0;
  overflow: hidden;
}

.filter-bar {
  width: 20%;
  flex-direction: column;
  height: 100%;
}
.content {
  width: 80%;
  flex-direction: column;
}
.product-list {
  margin: 5px;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}
.header-title {
  font-size: 30px;
  margin: 20px;
}
.order-product {
  margin-left: 20px;
}
.filter-bar {
  width: 20%;
  flex-direction: column;
  height: 100%;
}
.content {
  width: 80%;
  flex-direction: column;
}
.filter-input {
  background-color: #f2f2f2;
  border-width: 0px;
  color: rgb(155, 153, 155);
}

.searchBox {
  height: 40px;
  width: 200px;
  margin-top: 25px;
  right: 20px;

  padding-left: 10px;
}
.priceBox {
  height: 40px;
  width: 50px;

  padding-left: 10px;
}

::placeholder {
  color: rgb(155, 153, 155);
  padding-left: 10px;
}

.title {
  margin-bottom: 20px;
  margin-top: 20px;
  font-size: 24px;
}
.filter-check {
  background-color: #f2f2f2;
  flex-direction: column;
  margin-right: 50px;
  padding: 10px;
}

.show-btn {
  width: 207px;
  height: 25px;
  margin-top: 20px;
}
button:hover {
  color: tomato;
  border-width: 1px;
  border-color: tomato;
}

.header-title {
  font-size: 30px;
  margin: 20px;
}
</style>
