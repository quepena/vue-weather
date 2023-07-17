<template>
  <div class="about">
    <div v-for="(list, index) in listItems" v-if="listItems && listItems[0] && listItems[0].cod == '200'" class="card">
      <h1 v-if="list" class="city">Weather in {{ list.name }}</h1>
      <div v-for="item in list">
        <div v-if="item[0]" class="weather">{{ item[0].main }}</div>
        <div v-if="item[0]" class="weather">{{ item[0].description }}</div>
        <h2 v-if="item.temp" class="weather">{{ Math.floor(item.temp) }}</h2>
      </div>
      <div>
        <img v-bind:src="img[index]" alt="" />
      </div>
      <button class="buttons" v-on:click="deleteCity(list.name)">Delete From Favorites</button>
    </div>
  </div>
</template>

<style>
.search-input {
  padding: 10px;
  width: 30vw;
}

.card {
  margin-top: 10px;
  padding: 5px;
  border: 2px solid black;
  border-radius: 50px;
  height: 450px;
}

.buttons {
  background-color: white;
  border: 1px solid black;
  border-radius: 2px;
  height: 40px;
  width: 250px;
  font-size: 20px;
}
</style>

<script>
export default {
  data() {
    return {
      img: [],
      faves: localStorage.cities.split(','),
    }
  },
  methods: {
    async getData() {
      const cities = localStorage.cities.split(',');
      console.log(cities);
      for (let i = 0; i < 6; i++) {
        const res = await fetch(`https://api.openweathermap.org/data/2.5/weather?q=${cities[i]}&appid=${process.env.VUE_APP_API}&units=metric`);
        const finalRes = await res.json();
        if (finalRes.cod == '200') {
          this.listItems.push(finalRes);
          finalRes && finalRes.weather && finalRes.weather[0] && this.img.push(`https://openweathermap.org/img/wn/${finalRes.weather[0].icon}@2x.png`)
        }
      }
    },
  },
  beforeMount() {
    this.getData()
  }
}
</script>
