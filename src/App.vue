<template>
  <div class="app">
    <div :class="[{flexStart: step === 1}, 'wrapper']">
      <transition name="slide">
        <img src="./assets/logo.svg" alt="Spacer" class="logo" v-if="step === 1">
      </transition>
      <transition name="fade">
        <HeroImage v-if="step === 0" />
      </transition>
      <Claim  v-if="step === 0" />
      <SearchInput v-model="searchValue" @input="handleInput" :dark="step === 1"/>
      <div class="results">
        <Item 
          v-for="item in results" 
          :item="item" 
          :key="item.data[0].nasa_id"/>
      </div>
    </div>
  </div>
</template>

<script>
import Claim from '@/components/Claim';
import SearchInput from '@/components/SearchInput';
import HeroImage from '@/components/HeroImage';
import Item from '@/components/Item';
import axios from 'axios';
import debounce from 'lodash.debounce';

const API = 'https://images-api.nasa.gov/search';

export default {
  data() {
    return {
      loading: false,
      step: 0,
      searchValue: '',
      results: []
    }
  },
  components: {
    Claim,
    SearchInput,
    HeroImage,
    Item
  },
  methods: {
    handleInput: debounce(function() {
      this.loading = true;
      axios.get(`${API}?q=${this.searchValue}&media_type=image`)
        .then(res => {
         this.results = res.data.collection.items;
         this.loading = false;
         this.step = 1;
        })
        .catch(err => {
          console.log(err);
        })
    }, 500)
  }
}
</script>


<style lang="scss">
  @import url('https://fonts.googleapis.com/css?family=Montserrat:300,400,600,800');

  * {
    box-sizing: border-box;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }

  body {
    font-family: 'Montserrat', sans-serif;
    margin: 0;
    padding: 0;
  }

  .fade-enter-active, .fade-leave-active {
    transition: opacity .3s ease;
  }

  .fade-enter, .fade-leave-to {
    opacity: 0;
  }

  .slide-enter-active, .slide-leave-active {
    transition: margin-top .3s ease;
  }

  .slide-enter, .slide-leave-to {
    margin-top: -50px;
  }


  .wrapper {
    margin: 0;
    width: 100%;
    position: relative;
    min-height: 100vh;
    padding: 30px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    &.flexStart {
      justify-content: flex-start;
    }
  }

  .logo {
    position: absolute;
    top: 40px;
  }  

  .results {
    margin-top: 50px;
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-gap: 20px;

    @media (min-width: 768px) {
      width: 90%;
      grid-template-columns: 1fr 1fr 1fr; 
    }
  }
   
</style>
