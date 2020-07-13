<template>
  <div v-if="list.visible" class="card">
    <img :src="list.image[0]" :alt="list.name" class="card-img" />
    <p class="card-trade">{{list.trade}}</p>
    <p class="card-subtitle">{{list.name}}</p>
    <small>{{list.genderName}}- {{list.color}}</small>
    <small v-for="l in list.number" :key="l" class="number-view">{{l}}</small>
    <p class="card-price">{{list.price}}₺</p>
  </div>
</template>

<script>
export default {
  name: "Card",

  props: {
    list: Object
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.card {
  width: 300px;
  height: 500px;
  margin: 10px;
  text-align: center;
  border-radius: 16px;
}
.card-img {
  width: 300px;
  height: 300px;
  border-top-right-radius: 16px;
  border-top-left-radius: 16px;
}

.card-trade {
  font-size: 24px;
}
.card-subtitle {
  margin-top: 3px;
  font-size: 14px;
}

.card-price {
  margin-top: 5px;
  color: tomato;
}

div.card:hover {
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}
.number-view {
  margin: 2px;
  background-color: tomato;
  color: white;
  border-radius: 40px;
  padding: 1px;
}
</style>
