<template>
  <button :class="buttonClasses" @click="handleClick" ref="button" />
</template>

<script>
export default {
  name: "Tile",

  props: {
      index: {
          type: Number,
          required: true
      },
      name: {
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

  computed: {

  }, 

  methods: {
      play() {
          this.sound.play();
      },

      handleClick() {
          this.play();
      }
  },

  created() {
      const path = require('../assets/sounds/'+this.name);
      this.sound = new Audio(path);
  }
};
</script>

<style>
</style>