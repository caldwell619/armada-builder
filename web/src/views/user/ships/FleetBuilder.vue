<template lang='pug'>
  v-container
    v-row(justify='center' align='center')
      v-col(cols='6' md='5')
        FilterCards
      v-col(cols='6' md='5')
        SearchCards(@updateSearchTerm="updateSearchTerm")
    v-row
      v-col(cols='6' md='3' v-for="(ship, index) in availableShips" v-if="determineIsValidShip(ship)")
        ShipCard(:ship="ship")
</template>

<script>
import ShipCard from '@/components/ships/ShipCard.vue'
import FilterCards from '@/components/ships/FilterCards.vue'
import SearchCards from '@/components/ships/SearchCards.vue'
import { filterShips } from '@/utilities/filtering'
import { imperialShipCards } from '@/data/cards'
import { mapGetters } from 'vuex'
  export default {
    name: 'FleetBuilder',
    components: {
      ShipCard,
      FilterCards,
      SearchCards,
    },
    data(){
      return {
        searchTerm: '',
        imperialShipCards
      }
    },
    computed: {
      ...mapGetters('fleet', ['totalPoints']),
      availableShips(){
        return filterShips(imperialShipCards, this.searchTerm)
      }
    },
    methods: {
      updateSearchTerm(newTerm){
        this.updateSearchTerm = newTerm
      },
      determineIsValidShip(ship){
        return this.totalPoints + ship.points < 400
      }
    }
  }
</script>