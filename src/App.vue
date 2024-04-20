<script>
  
  import { ref } from 'vue';
  import axios from 'axios';
  import { computed } from 'vue';
  export default {
    data() {
      return  {
        gelenCagri: [],
        rastgeleNumara: null,
        searchQuery: '',
        selectedItemId: null,
        score: 0,
        isDisabled: false
      };
  },
  computed: {
    filteredItems() {
      return this.gelenCagri.filter(item => {
        return item.name.toLowerCase().includes(this.searchQuery.toLowerCase());
      });
    }
  },
  
  mounted() {
    
    this.rastgeleNumaraOlustur()
    axios.get('https://api.gameofthronesquotes.xyz/v1/characters')
    .then(response => {
      this.gelenCagri = response.data
      console.log(response.data[this.rastgeleNumara].name)
    })
    .catch(error => {
      console.log('Api basarisiz', error)
    })
  },

  methods: {
    rastgeleNumaraOlustur() {
      this.rastgeleNumara = Math.floor(Math.random() * 34)
      // let randomQuoteNumber = this.rastgeleQuoteNumara = Math.floor(Math.random() * this.gelenCagri[randomNumber].quotes.length)
      this.isDisabled = false
      this.searchQuery = ''
      return
    },
    selectItem(itemId) {
      console.log(this.selectedItemId = itemId)

      if (this.gelenCagri[this.rastgeleNumara].slug === itemId) {
        console.log("ayni")
        this.score += 10
        this.isDisabled = true
      } else {
        console.log("farkli")
        this.score -= 5
        this.isDisabled = true
      }
    }
  
  }
}
</script>

<template>
  <div>

    <!-- <h4>{{console.log(gelenCagri)}}</h4> -->
    <h4 v-if="gelenCagri < 1"> </h4>
    <div v-else class="cardBox"> 
        
      <h4 class="title">the quote is:</h4>
      
      <span class="quote">{{ gelenCagri[rastgeleNumara].quotes[0] }}</span>
      <button @click="rastgeleNumaraOlustur()">Değiştir</button>
      {{ console.log(rastgeleNumara) }}
    
      <div>
        <input type="text" v-model="searchQuery" placeholder="Aramak için yazın...">
        <ul>
          <li :class="{'disabled': isDisabled}" v-show="searchQuery" v-for="item in filteredItems" :key="item.slug" @click="selectItem(item.slug)">{{ item.name }}</li>
        </ul>
      <div class="score">
        Score is {{ score }}
      </div>
      </div>

      <h4 style="color:red; position: absolute; bottom: -50px;" > Cheat: {{ gelenCagri[rastgeleNumara].name }}</h4>

    </div> 
 </div>

</template>


<style>
body {
  background-color: rgb(245, 245, 245);
  color: #fff;
}

.cardBox {
  margin: auto auto;
  width:500px;
  background-color: rgb(36, 36, 36);
  margin-top: 50px;
  padding: 20px;
  border-radius: 20px;
  height: 550px;
  position: relative;
}
h4.title {
  font-size: 48px;
  font-weight: 300;
  font-style: italic;
}
span.quote {
  margin: 40px 20px 40px 20px;
  padding: 5px 20px 5px 20px;
  width: auto;
  display: block;
  font-size: 18px;
  position: relative;
  border-right: 5px solid #ddd;
}
span.quote::before {
  content: '``';
  position: absolute;
  top: -10px;
  left: -20px;
  font-size: 40px;
  color: #ddd;
  z-index: 2;
  font-weight: 500;
}
button {
  border: none;
  background-color: #ddd;
  border-radius: 10px 10px ;
  padding: 5px 10px;
  font-size: 16px;
  margin: 10px 0px 10px 0px;
}
input {
  background-color: #ddd;
  border: none;
}
ul {
  position: absolute;
  background-color:  rgb(36, 36, 36);
  padding: 20px;
  list-style-type: none;
  width: 200px;
  height: 200px;
  overflow: auto;
}
ul li {
  padding-bottom: 10px;
  transition: 300ms all ease-in-out;
}
ul li:hover {
  cursor: pointer;
  transform: scale(1.1);
}
.disabled {
  pointer-events: none; /* Tıklamayı devre dışı bırakır */
  opacity: 0.5; /* Opaklık ayarı */
}
.score {
  position: absolute;
  bottom: 20px;
  right: 20px;
  font-size: 24px;
}
</style>