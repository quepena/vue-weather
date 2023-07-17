<template>
  <div class="home">
    <div class="search">
      <input v-model="search" v-on:change="getData(search)" class="search-input" placeholder="Search">
    </div>
    <div v-for="(list, index) in listItems" v-if="listItems && listItems[0] && listItems[0].cod == '200'" class="card">
      <div>
        <h1 v-if="list" class="city">Weather in {{ list.name }}</h1>
        <div v-for="item in list">
          <div v-if="item[0]" class="weather">{{ item[0].main }}</div>
          <div v-if="item[0]" class="weather">{{ item[0].description }}</div>
          <h2 v-if="item.temp" class="weather">{{ Math.floor(item.temp) }}</h2>
        </div>
        <img v-bind:src="img[index]" alt="" />
        <!-- <div class="card">
          <Chart :search="list.name" />
        </div> -->
      </div>
      <div>
        <button class="buttons" v-on:click="deleteCity(index)">Delete</button>
        <button class="buttons" v-on:click="persist(list.name)" v-if="!faves.includes(list.name)">Add To
          Favorites</button>
      </div>
    </div>
    <Modal @modal="confirm($event, ix)" v-if="showModal" />
    <Modal2 @modal2="confirm2($event)" v-if="showModal2" />
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
  width: 200px;
  font-size: 20px;
}

.buttons:hover {
  background-color: lightgray;
}
</style>

<script>
// @ is an alias to /src
import Modal from '@/components/Modal.vue'
import Modal2 from '@/components/Modal2.vue'
import Chart from '@/components/Chart.vue'

export default {
  components: { Modal, Modal2, Chart },
  data() {
    return {
      listItems: [],
      img: [],
      showModal: false,
      showModal2: false,
      ix: null,
      faves: [],
    }
  },
  methods: {
    async getData(search) {
      if (this.listItems.length == 5 && search) {
        this.showModal2 = true
      } else if (this.faves.length == 5 && persist) {

      } else {
        const res = await fetch(`https://api.openweathermap.org/data/2.5/weather?q=${search}&appid=${process.env.VUE_APP_API}&units=metric`);
        const finalRes = await res.json();
        if (finalRes.cod == '200') {
          this.listItems.push(finalRes);
          finalRes && finalRes.weather && finalRes.weather[0] && this.img.push(`https://openweathermap.org/img/wn/${finalRes.weather[0].icon}@2x.png`)
        }
      }
    },
    confirm(evt, ix) {
      if (evt == "yes") {
        this.listItems.splice(ix, 1)
        this.img.pop(ix, 1)
      }
      this.showModal = false
    },
    deleteCity(ix) {
      this.showModal = true
      this.ix = ix
    },
    confirm2(evt) {
      console.log(evt);
      if (evt == "close") this.showModal2 = false
    },
    persist(city) {
      this.faves.push(city)
      localStorage.cities = this.faves;
    }
  },
}
</script>
