<template>
  <div>
    <h1 class="text-center b-3">{{ heading }}</h1>
    <button @click="toggleAllBreeds" class="btn btn-outline-secondary btn-lg">
      {{ breedmsg }}
    </button>
    <button @click="fetchRandomImg" class="btn btn-outline-secondary btn-lg">
      {{ randomImgMessage }}
    </button>
    <button @click="fetchByBreed" class="btn btn-outline-secondary btn-lg">
      {{ byBreedMsg }}
    </button>
    <img :src="breedRandomImg" v-if="showRandomImg" />
    <div v-if="showAllBreeds">
      <ul class="list-group">
        <li
          v-for="(item) of transformList"
          :key="item.name"
          class="list-group-item active">
          <div>{{ item.name }}</div>
          <div v-if="item.toggle">{{ item.breeds }}</div>
          <button @click="showSubList(item)" class="btn btn-outline-secondary btn-sm">{{bySubBreedmsg}}</button>
        </li>
      </ul>
    </div>
    <div v-if="showByBreed">
      <ul class="list-group">
        <li
          v-for="(item, index) in byBreed"
          :key="index"
          class="list-group-item active"
        >
          <img :src="item" />
        </li>
      </ul>
    </div>
    <div></div>
  </div>
</template>

<script>
export default {
  name: "dogceoApi",
  data() {
    return {
      heading: "DOG CEO",

      breedmsg: "All Breeds",
      showAllBreeds: false,

      bySubBreedmsg: "By Sub-Breeds",
      bySubBreed: "",
      transformList: [],

      randomImgMessage: "Random Image",
      breedRandomImg: false,
      showRandomImg: false,

      byBreedMsg: "By Breed",
      byBreed: false,
      showByBreed: false
    };
  },
  created() {
    fetch("https://dog.ceo/api/breeds/list/all")
      .then(response => response.json())
      .then(data => {
        // console.log(data);
        const objects = [];//created a variable that can store the data inside the for loop
        for(let name in data.message) {
          objects.push({
            toggle: false, // toggle is key and false is value
            name: name,
            breeds: data.message[name] //returns value for a specific key 
          }) //we loop through object list
        }
        this.transformList = objects; // transformList has been identified on the data as an empty array
        console.log(objects);
      });
  },
  methods: {
    toggleAllBreeds() {
      this.showAllBreeds = !this.showAllBreeds;
      this.showRandomImg = false;
      this.showByBreed = false;
   },
   showSubList(data) {
     data.toggle = !data.toggle;
     console.log(data);
   },
    fetchRandomImg() {
      fetch("https://dog.ceo/api/breeds/image/random")
        .then(response => response.json())
        .then(data => {
          this.breedRandomImg = data.message;
          this.showAllBreeds = false;
          this.showByBreed = false;
          this.showRandomImg = !this.showRandomImg;
        });
    },
    fetchByBreed() {
      fetch("https://dog.ceo/api/breed/rottweiler/images")
        .then(response => response.json())
        .then(data => {
          this.byBreed = data.message.slice(0, 3);
          this.showAllBreeds = false;
          this.showRandomImg = false;
          this.showByBreed = !this.showByBreed;
        });
    },
  }
};
</script>
<style>
body {
  margin: auto 0;
  background-color: black;
}
button,
.btn {
  margin: 10px;
}
</style>
