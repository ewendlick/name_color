<template>
  <div id="app" :style="{ 'background-color': resultColorHex }">
    <div class="interaction-container">
      <h1>Please enter your name</h1>
      <input v-model="name" class="name-input" type="text" autofocus />

      <h2>Result</h2>
      <p>The sum of all character code values is: {{ characterCodeSum || 'Currently unknown' }}</p>
      <p v-show="resultColorName">This value is modded by {{ modValue }} to get the "{{ resultColorName }}" color</p>
    </div>

    <div class="available-colors-container">
      <h2>Available Colors</h2>
      <div class="color-container">
        <div v-for="color in colors"
            :key="color.hex"
            class="color"
            :class="{ 'light-text': color.isLightText }"
            :style="{ 'background-color': color.hex }">
          <div class="hex">{{ color.hex }}</div>
          <div class="name">{{ color.name }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// import { ref } from 'vue' // For using the composition API

export default {
  name: 'App',
  data () {
    return {
      name: null
    }
  },
  computed: {
    characterCodeSum () {
      if (this.name === null || this.name.length === 0) return null

      return this.name.split('').reduce((accumulator, character) => character.charCodeAt(0) + accumulator, 0)
    },
    modValue () {
      return this.colors.length
    },
    resultColor () {
      // const { characterCodeSum, modValue } = this
      if (this.characterCodeSum === null) return

      const colorIndex = this.characterCodeSum % this.modValue
      return this.colors[colorIndex]
    },
    resultColorName () {
      if (!this.resultColor) return

      return this.resultColor.name
    },
    resultColorHex () {
      if (!this.resultColor) return '#fff'

      return this.resultColor.hex
    }
  },
  created () {
    this.colors = [
      {
        hex: '#8da1b9',
        isLightText: true, // TODO: generate
        name: 'Cadet Grey'
      },
      {
        hex: '#95adb6',
        isLightText: false,
        name: 'Pewter Blue'
      },
      {
        hex: '#cbb3bf',
        isLightText: false,
        name: 'Thistle'
      },
      {
        hex: '#dbc7be',
        isLightText: false,
        name: 'Champagne Pink'
      },
      {
        hex: '#ef959c',
        isLightText: false,
        name: 'Salmon Pink'
      }
    ]
  }
}
</script>

<style>
body, html {
  margin: 0;
  width: 100%;
  height: 100%;
}

#app {
  width: 100%;
  height: 100%;

  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;

  background-color: #fff;
  transition: background-color 1s;
}

.interaction-container {
  padding: 24px;
}

.interaction-container h1 {
  font-size: 48px;
}

.interaction-container h2 {
  font-size: 32px;
}

.interaction-container p {
  font-size: 24px;
}

.interaction-container input[type="text"] {
  font-size: 60px;
  border-color: transparent;
  border-bottom: #111 solid 2px;
  outline: none;
  background-color: transparent;
  margin-bottom: 32px;
}

.available-colors-container .color-container {
  display: flex;
  width: 100%;

  border-top: #111 solid 2px;
  border-bottom: #111 solid 2px;
}

.available-colors-container .color-container .color {
  width: 100%;
  height: 100px;

  color: #111;

  padding: 16px;
}

.available-colors-container .color-container .color.light-text {
  color: #efefef;
}

.available-colors-container .color-container .color .hex {
  font-size: 24px;
}

.available-colors-container .color-container .color .name {
  font-size: 16px;
}
</style>
