<template>
  <Header title="The Office Gif Quiz" />
  <div class="container">
    <div
      class="drop-zone"
      @drop="OnDrop($event, `gifs`)"
      @dragenter.prevent
      @dragover.prevent
    >
      <Gif
        v-for="gif in GetList()"
        :key="gif.id"
        :gif="gif"
        class="drag-el"
        draggable="true"
        @dragstart="StartDrag($event, gif)"
      />
    </div>
    <div class="drop-zone">
      <div
        v-for="season in seasons"
        :key="seasons.indexOf(season)"
        :season="season"
        :id="seasons.indexOf(season)"
        class="season"
        @drop="OnDrop($event, season)"
        @dragenter.prevent
        @dragover.prevent
      >
        <Gif
          v-for="gif in GetList(season)"
          :key="gif.id"
          :gif="gif"
          class="drag-el"
          @dragstart="StartDrag($event, gif)"
          draggable="true"
        />
        <h3>{{ season }}</h3>
      </div>
    </div>
    <Button @check-score="GetResults" />
    <Popup v-if="modalOpen">
      <h2>Your Score Is</h2>
      <p>{{ score }}/{{ gifs.length }}</p>
    </Popup>
  </div>
</template>

<script>
import axios from "axios";
import Header from "./components/Header.vue";
import Gif from "./components/Gif.vue";
import Button from "./components/Button.vue";
import Popup from "./components/Popup.vue";

export default {
  name: "App",
  components: {
    Header,
    Gif,
    Button,
    Popup,
  },
  data() {
    return {
      gifs: [],
      seasons: [],
      score: 0,
      modalOpen: false,
    };
  },
  methods: {
    async GetGif() {
      const apiKey = "Gc7131jiJuvI7IdN0HZ1D7nh0ow5BU6g";
      const userName = "%40theoffice";
      let offset = Math.floor(Math.random() * 2000);
      let url = `https://api.giphy.com/v1/gifs/search?offset=${offset}&limit=1&q=${userName}&api_key=${apiKey}`;
      let response = await axios.get(url);
      while (!response.data.data[0]) {
        offset = Math.floor(Math.random() * 2000);
        url = `https://api.giphy.com/v1/gifs/search?offset=${offset}&limit=1&q=${userName}&api_key=${apiKey}`;
        response = await axios.get(url);
      }
      let result = {
        id: response.data.data[0].id,
        type: response.data.data[0].type,
        image: response.data.data[0].images.original.url,
      };

      const tags = response.data.data[0].tags;
      let seasonFound = false;
      tags.forEach(tag => {
        if (tag.includes("season") && !seasonFound) {
          result = {
            ...result,
            season: tag,
            list: "gifs",
          };
          seasonFound = true;
        }
      });

      if (!this.seasons.includes(result.season)) {
        const season = result.season;
        this.seasons.push(season);
      }
      this.gifs.push(result);
    },
    GetSeasons() {
      return this.seasons;
    },
    GetList(_str = "gifs") {
      return this.gifs.filter(gif => gif.list === _str);
    },
    StartDrag(event, item) {
      event.dataTransfer.dropEffect = "move";
      event.dataTransfer.effectAllowed = "move";
      event.dataTransfer.setData("gifID", item.id);
    },
    OnDrop(event, list) {
      const gifID = event.dataTransfer.getData("gifID");
      const gif = this.gifs.find(gif => gif.id === gifID);
      gif.list = list;
    },
    GetResults() {
      let result = 0;
      this.gifs.forEach(gif => {
        if (gif.list === gif.season) {
          result++;
        }
      });
      this.score = result;
      this.modalOpen = true;
    },
  },
  created() {
    let numGifs = 1;
    while (numGifs <= 4) {
      this.GetGif();
      numGifs++;
    }
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  font-family: Arial;
}
body {
  display: flex;
  justify-content: center;
  min-height: 100vh;
}
.container {
  margin: 10px;
  border: 4px solid black;
}
.drop-zone {
  display: grid;
  grid-template-columns: auto auto;
  border: 2px solid black;
  padding: 5px;
  margin: 5px;
  min-height: 30%;
}

.drag-el {
  padding: 2px;
}

.season {
  display: flex;
  color: #ffff;
  min-width: 200px;
  height: 80px;
  text-align: center;
  text-transform: uppercase;
  padding: 2px;
  margin: 5px;
  background: #ff9933;
}

.season h3 {
  border: 2px solid white;
  position: absolute;
  align-self: flex-start;
}

.season img {
  align-self: flex-end;
  width: 60px;
}

img {
  width: 230px;
  max-width: 100%;
}

p {
  text-align: center;
  font-size: 6rem;
  letter-spacing: 3px;
}
</style>
