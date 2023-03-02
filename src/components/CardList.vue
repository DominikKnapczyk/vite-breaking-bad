<script>
import axios from "axios";

export default {
  data() {
    return {
      // array che conterrà le carte scaricate
      cards: [],
      // array che conterrà le carte scaricate
      selectedArchetype: "",
       // array che conterrà tutti gli archetipi presenti nelle carte scaricate
      archetypes: []
    };
  },

  // fetchArchetypes viene eseguita quando il componente viene creato
  created() {
    this.fetchArchetypes();
  },

  // watch serve a monitorare i cambiamenti della variabile selectedArchetype
  watch: {
    selectedArchetype: function() {
      this.fetchCardsByArchetype();
    }
  },

  methods: {
    // metodo per scaricare le carte in base all'archetipo selezionato
    fetchCardsByArchetype() {
      axios
        .get("https://db.ygoprodeck.com/api/v7/cardinfo.php", {
          params: {
            num: 500,
            offset: 0,
            archetype: this.selectedArchetype
          }
        })
        .then((response) => {
          this.cards = response.data.data;
        });
    },

    fetchArchetypes() {
      axios
        .get("https://db.ygoprodeck.com/api/v7/cardinfo.php")
        .then((response) => {
          const allCards = response.data.data;
          const archetypesSet = new Set();
           // scorre tutte le carte e ne estrae l'archetipo (se presente)
          allCards.forEach((card) => {
            if (card.archetype) {
              archetypesSet.add(card.archetype);
            }
          });
          // converte il Set in un array e lo assegna alla variabile archetypes
          this.archetypes = Array.from(archetypesSet);
        });
    }
  }
};
</script>

<template>
  <main>
    <div class="container-1">
      <select v-model="selectedArchetype">
        <option value="">-- Seleziona un Archetype --</option>
        <option v-for="archetype in archetypes" :value="archetype">{{ archetype }}</option>
      </select>
      <div class="container-2">
        <div class="container-3">
          <div class="found-card-display">
            <span>Found {{ cards.length }} cards</span>
          </div>
          <div class="cards-container">
            <div class="single-card" v-for="(card, index) in cards" :key="card.id">
              <img :src="card.card_images[0].image_url" alt="card">
              <h3>{{ card.name }}</h3>
              <h4>{{ card.archetype }}</h4>
            </div>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<style lang="scss" scoped>

main {
  display: flex;
  justify-content: center;
  align-items: center;

}
.container-1 {
  width: 1200px;
  
  select {
    padding: 10px 0px;
    margin: 30px 0 30px 20px;
  }
  .container-2 {
    width: 1200px;
    padding: 50px 0;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: white;

    .container-3 {
      width: 1100px;
      .found-card-display {
        display: flex;
        align-items: center;
        height: 55px;
        background-color: #212529;
        color: white;
        font-weight: bold;

        span {
          padding-left: 20px;
        }
      }

      .cards-container {
        display: grid;
        grid-template-columns: repeat(5, 1fr);
        grid-gap: 20px;
        justify-content: center;
        align-items: center;
        margin: 0 auto;
        padding: 0;
        .single-card {
          width: 200px;

          display: flex;
          flex-direction: column;
          text-align: center;
          
          background-color: #D48F38;

          h3 {
            color: white;
            font-size: 24px;
            font-family:'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
            font-weight: bold;

            height: 120px;
            padding: 20px 0;
          }

          h4 {
            font-size: 22px;
            font-weight: 400;

            height: 30px;
            padding: 0 0 20px 0;
          }
        }
      }
    }
  }  
}
</style>

<!-- {
    "id": 34541863,
    "name": "\"A\" Cell Breeding Device",
    "type": "Spell Card",
    "frameType": "spell",
    "desc": "During each of your Standby Phases, put 1 A-Counter on 1 face-up monster your opponent controls.",
    "race": "Continuous",
    "archetype": "Alien",
    "card_sets": [
        {
            "set_name": "Force of the Breaker",
            "set_code": "FOTB-EN043",
            "set_rarity": "Common",
            "set_rarity_code": "(C)",
            "set_price": "1.31"
        }
    ],
    "card_images": [
        {
            "id": 34541863,
            "image_url": "https://images.ygoprodeck.com/images/cards/34541863.jpg",
            "image_url_small": "https://images.ygoprodeck.com/images/cards_small/34541863.jpg",
            "image_url_cropped": "https://images.ygoprodeck.com/images/cards_cropped/34541863.jpg"
        }
    ],
    "card_prices": [
        {
            "cardmarket_price": "0.17",
            "tcgplayer_price": "0.19",
            "ebay_price": "4.99",
            "amazon_price": "24.45",
            "coolstuffinc_price": "0.25"
        }
    ]
} -->