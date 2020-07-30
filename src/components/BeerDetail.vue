<template>
  <div v-if='beer'>
      <h3>Name: {{ beer.name}}</h3>
      <h4>Tag: {{ beer.tagline }}</h4>
      
      <div id="favourite-button">
            <button v-if="!favBeers.includes(beer)" v-on:click="handleClick">Add {{beer.name}} to your favourite beers?</button>
            <button v-if="favBeers.includes(beer)" v-on:click="handleRemove">Remove {{beer.name}} from your favourite beers?</button>
      </div>
      <img :src="beer.image_url">

      <div id="ingredients">
          <h4>Malts</h4>
          <ul>
              <li v-for="(malt, index) in beer.ingredients.malt" :malt="malt" :key="index">{{malt.name}}</li>
          </ul>
          <h4>Hops</h4>
            <ul >
              <li v-for="(hop, index) in filterDups" :hop="hop" :key="index">{{hop.name}}</li>
            </ul>
            <h4>Yeast</h4>
            <ul>
                <li>{{beer.ingredients.yeast}}</li>
            </ul>
      </div>

      
  </div>
</template>

<script>
import {eventBus} from '@/main.js';
import FavouriteBeers from './FavouriteBeers.vue';

export default {
    name: 'beer-detail',
    data() {
        return {
            beer: null,
            favBeers: []
        }
    },
    mounted() {
        eventBus.$on('beer-select', (beer) => {this.beer = beer}),
        eventBus.$on('favourite-beers', (beers) => {this.favBeers = beers})
    },
    props: ['favourite-beer'],
    computed: {
        filterDups() {
            const list = this.beer.ingredients.hops;
            // return [...new Set(list.name)];
            console.log(list);
            const newList = list.filter((value, index) => list.indexOf(value) === index);
            console.log(newList);
            return newList;
        }
    },
    methods: {
        handleClick() {
            eventBus.$emit('favourite-beer', this.beer)
        },
        handleRemove() {
            eventBus.$emit('unfavourite-beer', this.beer)
        },
    },
    components: {
        'favourite-beers' : FavouriteBeers
    }
    

}
</script>

<style>
    img {
        width: 125px;
        height: 300px;
    }
</style>