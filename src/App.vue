<template>
  <div class="weather" :class="weatherClass">
    <div class="container">
      <div class="card weather-form">
        <input type="text" class="weather-form_input" v-model="searchQuery" @keyup.enter="weatherSearch" placeholder="Enter city">
        <button class="weather-form_btn" @click="weatherSearch"
        
        >Search</button>
      </div>

      <div class="card weather-load" v-if="loading">Loading...</div>
      <div class="weather-info" v-show="!error && location && temperature !== 0 && description">

        <div class="card" v-if="error">Error</div>

        <div class="weather-info_text">
          <p class="card">{{location}}</p>
          <p class="card">{{temperature}}</p>
          <p class="card">{{description}}</p>

        </div>
      </div>
    </div>
    <div class="weather-bg">
      <div>
        <img class="weather-bg__img bg" src="./assets/bg.jpg" alt="">
        <img class="weather-bg overcast" src="./assets/sunny.jpg" alt="">
        <img class="weather-bg partly-cloudy" src="./assets/rainy.jpg" alt="">
        <img class="weather-bg sunny" src="./assets/cloudy.jpg" alt="">
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      location:'',
      temprature:'0',
      description:'',
      loading: false,
      error: false,
      searchQuery: '',
    };
  },
  computed: {
  weatherClass(){
    if(this.description.includes('Sunny')){
      return 'sunny';
    } else if(this.description.includes('Overcast')){
      return 'overcast';
    } else if(this.description.includes('Partly cloudy')){
      return 'partly-cloudy';
    }
  }

  },
  methods: {
   weatherSearch() {
     this.loading = true;
     this.error = false;
     fetch(`https://api.weatherapi.com/v1/current.json?key=69ed7b91ab4b4d4f9f282327242604&q=${this.searchQuery}`)
       .then(response => response.json())
       .then(data => {
         this.loading = false;
         this.location = data.location.name;
         this.temperature = data.current.temp_c;
         this.description = data.current.condition.text;
         this.resetSearchQuery();
       })
       .catch(error => {
         this.loading = false;
         this.error = true;
         console.error(error);
       });
   },
   resetSearchQuery() {
     this.searchQuery = '';
   }
 },
    resetSearchQuerty(){

    }
  };
</script>