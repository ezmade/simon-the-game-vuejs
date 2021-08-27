<template>
  <input type="button" :class="tileClasses" @click="handleClick" ref="tile" />
</template>

<script>
export default {
  name: "Tile",
  props: {
      indx: {
          type: Number,
          required: true
      },
      file: {
          type: String,
          required: true
      },
      color: {
          type: String,
          required: true
      }
  },

  data() {
    return {
      sound: null,
    };
  },

  created() {
      const path = require('../assets/sounds/'+this.file);
      this.sound = new Audio(path);
  },

  computed: {
    tileClasses() {
			return ['tile', this.color];
		},
  }, 

  methods: {

      play() {
          this.sound.play();
          this.$refs.tile.classList.add('active');
          setTimeout(() => {
            this.$refs.tile.classList.remove('active');
          }, 400);
      },

      handleClick() {
        this.play();
        this.$emit('click', this.indx)
      }
  },
};
</script>

<style scoped>
  .tile {
    width: inherit;
		height: inherit;
		border: none;
  }

  .tile:focus {
    box-shadow: inset 0px 0px 50px 0 rgba(0, 0, 0, 0.25);
		outline: none
  }

  .tile.red {
    background-color: tomato;
  }
  .tile.red.active {
    background-color: red;
  }
  .tile.green {
    background-color: darkseagreen;
  }
  .tile.green.active {
    background-color: green;
  }

  .tile.blue {
    background-color: royalblue;
  }
  .tile.blue.active {
    background-color: blue;
  }

  .tile.yellow {
    background-color: khaki;
  }
  .tile.yellow.active {
    background-color: yellow;
  }
</style>