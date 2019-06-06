<template>
  <div id="app">

    <div class="filtros">

      <label>
        <input type="radio" name="all" value= "0" v-model="filterVal" checked>
        Todos los albumes
      </label>
      <label>
        <input type="radio" name="ff" value= "1" v-model="filterVal">
        Foo Fighters
      </label>
      <label>
        <input type="radio" name="rhcp" value= "2" v-model="filterVal">
        RHCP
      </label>
      <label>
        <input type="radio" name="qotsa" value= "3" v-model="filterVal">
        QOTSA
      </label>
      <label>
        <input type="radio" name="gorillaz" value= "4" v-model="filterVal">
        Gorillaz
      </label>

      <label>
        <input type="radio" name="allPrices" value="rangeTd" v-model="rangeFilter" checked>
        Todos los precios
      </label>
      <label>
        <input type="radio" name="first" value="range1" v-model="rangeFilter">
          $1 - $10
      </label>
      <label>
        <input type="radio" name="second" value="range2" v-model="rangeFilter">
          $11 - $20
      </label>
      <label>
        <input type="radio" name="third" value="range3" v-model="rangeFilter">
          $21 - $30
      </label>
      <label>
        <input type="radio" name="fourth" value="range4" v-model="rangeFilter">
          Sobre $30
      </label>

    </div>

    <div class="display">
      <div class="cards" v-for="(product, idx) in mainFilter" :key="idx">
        <h3>{{product.val}}</h3>
        <small>{{product.by}}</small>
        <small>{{product.year}}</small>
        <img :src="product.image" alt="image" class="img-responsive">
        <div class="price"><small>${{product.price}}</small></div>
      </div>
    </div>

  </div>
</template>

<script>

import json from './json/bands.json';

export default {
  name: "app",
  methods: {
    getAlbumsByPrice: function(e){
      if(this.rangeFilter === "rangeTd"){
        return e.filter(album => album.price > 0);
      }else if(this.rangeFilter === "range1"){
        return e.filter(album => album.price>=0 && album.price<=10);
      }else if(this.rangeFilter === "range2"){
        return e.filter(album => album.price>10 && album.price<=20);
      }else if(this.rangeFilter === "range3"){
        return e.filter(album => album.price>20 && album.price<=30);
      }else if(this.rangeFilter === "range4"){
        return e.filter(album => album.price>30);
      }else{
        return [];
      }
    }
  },
  computed:{
    getAlbumsByBand: function(){
      if(this.filterVal === " " || parseInt(this.filterVal) === 0){
        return this.bands.filter(album => album.parent != 0);
      }else if(this.filterVal != 0){
        return this.bands.filter(album => album.parent === parseInt(this.filterVal));
      }
    },
    mainFilter: function(){ 
      if(this.getAlbumsByBand.length > 0){
        if(this.rangeFilter==="rangeTd"||this.rangeFilter==="range1"||this.rangeFilter ==="range2"||this.rangeFilter ==="range3"||this.rangeFilter ==="range4"){
          return this.getAlbumsByPrice(this.getAlbumsByBand);
        }
        return this.getAlbumsByBand;
      }
    }
  },
  data: function(){
    return {
      bands: json,
      filterVal: " ",
      rangeFilter: " "
    }
  }
};
</script>

<style lang="scss">
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  flex-direction: row;
}

.filtros{
  width: 10%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  text-align: start;
  margin: 5px;

  border: solid 2px black;
}

.display{
  width: 90%;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  margin: 20px;
}

.cards{
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
  justify-content: space-between;
  width: 150px;
  margin: 10px;
  border: solid 1px black;
  h3{
    font-size: .8rem;
    line-height: .8rem;
    text-transform: uppercase;
    margin: 4px 0 4px 0;
  }
  .price{
    display: flex;
    justify-content: flex-end;
    small{
      margin: 0 .4rem .2rem 0;
    }
  }
    img{
    width: 100%;
    height: auto
  }
}


</style>
