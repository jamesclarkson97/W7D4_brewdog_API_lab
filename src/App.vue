<template>
  <div>
    <beer-select :beers="beers"/>
    <beer-detail/>
    <favourite-beers/>
  </div>
</template>

<script>
import BeerSelect from './components/BeerSelect.vue';
import BeerDetail from './components/BeerDetail.vue';
import FavouriteBeers from './components/FavouriteBeers.vue';

export default {
  name: 'app',
  data() {
    return {
      beers: []
    };
  },
  mounted() {
    fetch('https://api.punkapi.com/v2/beers')
    .then(response => response.json())
    .then(data => this.beers = data)
  },
  components: {
    "beer-select": BeerSelect,
    "beer-detail": BeerDetail,
    "favourite-beers": FavouriteBeers
  }
}
</script>

<style>

</style>