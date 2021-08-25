<template>
  <div class="game">
    <section class="field">
      <ul class="tiles">
        <li v-for="(tile, index) in tiles" :key="index" class="tile">
          <Tile :index="index" :color="tile.color" :name="tile.file" ref="button" @click="handleTileClick" />
        </li>
      </ul>
    </section>

    <section class="scores">

    </section>

    <section class="settings">
      <h3 class="small-title">Сложность</h3>
      <ul class="difficulty-list">
        <li v-for="(item, index) in dificultyLevels" :key="index" class="difficulty-item">
          <input type="radio" v-model="settings.difficulty" :value="item.value" id="difficulty-${item.value}" class="difficulty-radio-input" />
          <label :for="'difficulty-${item.value}'">{{ item.label }}</label>          
        </li>
      </ul>
    </section>
  </div>
</template>

<script>
import Tile from './Tile.vue'

export default {
  name: 'Game',
  components: {
    Tile
  },
  data() {
    return {
      game: {
        isOver: false,
        round: 0,
        current: 0,
        gameSequence: [],
        currentTile: null
      },
      settings: {
        difficulty: "easy",
        interval: 1500
      },

      dificultyLevels: [
        {
          value: "easy",
          label: "Легко",
          interval: 1500
        },
        {
          value: "medium",
          label: "Средне",
          interval: 2000
        },
        {
          value: "hard",
          label: "Сложно",
          interval: 2500
        }
      ],

      tiles: [
        {
          color: "red",
          file: "red.mp3"
        },
        {
          color: "green",
          file: "green.mp3"
        },
        {
          color: "blue",
          file: "blue.mp3"
        },
        {
          color: "yellow",
          file: "yellow.mp3"
        }
      ]
    }
  }, 
  methods: {
    handleTileClick(index) {
      console.log('clicked on index: ', index);
    }
  }
}
</script>

<style>
.settings {
  display: grid;
  text-align: left;
}

.difficulty-item {
  list-style-type: none;
}
</style>
