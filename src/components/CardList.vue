<script>
import axios from "axios";

export default {
  data() {
    return {
      cards: [

      ],
    };
  },
  created() {
  axios
    .get("https://db.ygoprodeck.com/api/v7/cardinfo.php", {
      params: {
        num: 15,
        offset: 2,
        archetype: "alien"
      }
    })
    .then((response) => {
      this.cards = response.data.data;
    });
},
};
</script>

<template>
  <div class="cards-container">
    <div class="single-card" v-for="(card, index) in cards" :key="card.id">
      <img :src="card.card_images[0].image_url" alt="card">
      <h3>{{ card.name }}</h3>
      <h4>{{ card.archetype }}</h4>
    </div>
  </div>
</template>

<style lang="scss" scoped>
  .cards-container {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    grid-template-rows: repeat(3, 1fr);
    grid-gap: 20px;
    justify-content: center;
    align-items: center;
    margin: 0 auto;
    max-width: 1200px;
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