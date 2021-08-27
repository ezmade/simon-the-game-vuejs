<template>
  <div class="game">
    <section class="field">
      <ul class="tiles">
        <li v-for="(item, indx) in tiles" :key="indx" class="tile">
          <Tile
            :indx="indx"
            :color="item.color"
            :file="item.file"
            :ref="this.setTileRef"
            @click="handleTileClick"
          />
        </li>
      </ul>
      <button class="btn" @click="startGame">Start</button>
      <span v-show="game.isOver" class="result">Вы проиграли после {{ game.round }} раундов!</span>
    </section>

    <section class="scores"></section>

    <section class="settings">
      <h3 class="small-title">Сложность</h3>
      <ul class="difficulty-list">
        <li
          v-for="(item, indx) in dificultyLevels"
          :key="indx"
          class="difficulty-item"
        >
          <input
            type="radio"
            v-model="settings.difficulty"
            :value="item.value"
            :id="item.value"
            class="difficulty-radio-input"
          />
          <label :for="item.value">{{ item.label }}</label>
        </li>
      </ul>
    </section>
  </div>
</template>

<script>
import Tile from "./Tile.vue";

export default {
  name: "Game",
  components: {
    Tile,
  },
  data() {
    return {
      game: {
        isOver: false,
        tileRefs: [],
        round: 0,
        current: 0,
        sequence: [],
        currentTile: null,
      },
      settings: {
        difficulty: "easy",
      },

      dificultyLevels: {
        easy: {
          value: "easy",
          label: "Легко",
          interval: 1500,
        },
        medium: {
          value: "medium",
          label: "Средне",
          interval: 1000,
        },
        hard: {
          value: "hard",
          label: "Сложно",
          interval: 400,
        },
      },

      tiles: [
        {
          color: "red",
          file: "red.mp3",
        },
        {
          color: "green",
          file: "green.mp3",
        },
        {
          color: "blue",
          file: "blue.mp3",
        },
        {
          color: "yellow",
          file: "yellow.mp3",
        },
      ],
    };
  },

  methods: {
    getInterval() {
      return this.dificultyLevels[this.settings.difficulty].interval;
    },

    sleep(ms) {
      return new Promise((resolve) => setTimeout(resolve, ms));
    },

    setTileRef(el) {
      if (el) {
        this.game.tileRefs.push(el);
      }
    },

    startGame() {
      this.resetGameProgress();
      this.makeStep();
    },

    resetGameProgress() {
      this.game.round = 0;
      this.game.isOver = false;
      this.game.sequence = [];
      this.game.currentTile = null;
    },

    async makeStep() {
      this.game.round++;
      this.game.current = 0;

      const tile = this.generateNextTile();
      this.game.currentTile = tile;
      this.game.sequence.push(tile);

      for (const indx of this.game.sequence) {
        this.game.tileRefs[indx].play();
        await this.sleep(this.getInterval());
      }
    },

    generateNextTile() {
      let indx;
      do {
        indx = Math.round(Math.random() * 3);
      } while (indx === this.game.currentTile);
      return indx;
    },

    handleTileClick(indx) {
      if (!this.game.round) return;

      const currentIndex = this.game.sequence[this.game.current];
      console.log(this.game.isOver)
      if (indx === currentIndex) {
        this.addCurrent();
      } else {
        this.game.isOver = true;
      }
    },

    async addCurrent() {
      if (this.game.current !== this.game.round - 1) {
        this.game.current++;
      } else {
        await this.sleep(1500);
        this.makeStep();
      }
    },
  },
};
</script>

<style>
.settings {
  display: grid;
  text-align: left;
}

.difficulty-item {
  list-style-type: none;
}

.tiles {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: 1fr 1fr;
  padding: 0;
  margin: 2rem;
  height: 75vw;
  width: 75vw;
  max-height: 500px;
  max-width: 500px;
  list-style-type: none;
}

.tile {
  display: block;
  height: 100%;
  width: 100%;
  overflow: hidden;
}
</style>
