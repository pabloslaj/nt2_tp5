<template>
  <div>
    <div id="header" :style="{ backgroundColor: backgroundColor }">
      <h1>
        The Great <br />
        <span id="colorDisplay">{{ pickedColor }}</span>
        <br />
        Guessing Game
      </h1>
    </div>

    <div id="navigator">
      <button id="reset" @click="restart">{{ restartButton }}</button>
      <span id="message">{{ mensaje }}</span>

      <button id="easy" @click="easy">easy</button>
      <button id="hard" class="selected" @click="hard">hard</button>
    </div>

    <div id="container">
      <div
        class="square"
        :style="{ 'background-color': this.colors[0] }"
        @click="validar(0)"
      ></div>
      <div
        class="square"
        :style="{ 'background-color': this.colors[1] }"
        @click="validar(1)"
      ></div>
      <div
        class="square"
        :style="{ 'background-color': this.colors[2] }"
        @click="validar(2)"
      ></div>
      <div
        class="square"
        :style="{ 'background-color': this.colors[3] }"
        @click="validar(3)"
        v-if="isHard"
      ></div>
      <div
        class="square"
        :style="{ 'background-color': this.colors[4] }"
        @click="validar(4)"
        v-if="isHard"
      ></div>
      <div
        class="square"
        :style="{ 'background-color': this.colors[5] }"
        @click="validar(5)"
        v-if="isHard"
      ></div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Colors",
  mounted() {
    (this.colors = this.createNewColors()), this.start();
  },
  data() {
    return {
      colorCount: 6,
      isHard: true,
      colors: [],
      pickedColor: 0,
      restartButton: "New Colors",
      mensaje: "",
      backgroundColor: "steelblue",
      squares: document.querySelectorAll(".square"),
      colorDisplay: document.getElementById("colorDisplay"),
      easyButton: document.querySelector("#easy"),
      hardButton: document.querySelector("#hard"),
    };
  },
  methods: {
    start() {
      if (this.isHard) {
        this.colors = this.createNewColors(6);
      } else {
        this.colors = this.createNewColors(3);
      }
      console.log(this.colors);
      this.pickedColor = this.colors[this.PickColor()];
    },
    PickColor() {
      var quantity;

      if (this.isHard) {
        quantity = 6;
      } else {
        quantity = 3;
      }
      return Math.floor(Math.random() * quantity);
    },
    setAllColorsTo() {
      let squares = document.querySelectorAll(".square");

      for (let i = 0; i < this.colorCount; i++) {
        squares[i].style.backgroundColor = this.pickedColor;
      }
    },

    createNewColors(numbers) {
      var arr = [];
      for (var i = 0; i < numbers; i++) {
        arr.push(this.createRandomStringColor());
      }
      return arr;
    },

    createRandomStringColor() {
      var newColor =
        "rgb(" +
        this.randomInt() +
        ", " +
        this.randomInt() +
        ", " +
        this.randomInt() +
        ")";
      console.log(newColor);
      return newColor;
    },

    randomInt() {
      return Math.floor(Math.random() * 256);
    },

    restart() {
      this.colors = this.createNewColors();
      this.pickedColor = this.colors[this.PickColor()];

      this.restartButton = "New Colors!";
      this.mensaje = "";
      this.backgroundColor = "steelblue";

      this.start();
    },

    easy() {
      if (this.isHard) {
        this.isHard = false;
        this.colorCount = 3;
        document.querySelector("#hard").classList.remove("selected");
        document.querySelector("#easy").classList.add("selected");
        this.restart();
      }
    },

    hard() {
      if (!this.isHard) {
        this.isHard = true;
        this.colorCount = 6;
        document.querySelector("#easy").classList.remove("selected");
        document.querySelector("#hard").classList.add("selected");
        this.restart();
      }
    },

    validar(posicion) {
      console.log(posicion);

      if (this.colors[posicion] == this.pickedColor) {
        this.setAllColorsTo(this.colors[posicion]);
        this.mensaje = "You Picked Right!";
        this.restartButton = "Play Again!";
        this.backgroundColor = this.pickedColor;
      } else {
        document.querySelectorAll(".square")[posicion].style.backgroundColor =
          "#232323";
        this.mensaje = "Try Again!";
      }
    },
  },
};
</script>

<style>
body {
  background: #232323;
  margin: 0;
  font-family: "Montserrat", "Avenir";
}

h1 {
  font-weight: normal;
  line-height: 1.1;
  padding: 20px 0;
}

#navigator {
  background: #ffffff;
  height: 30px;
  text-align: center;
  margin: 0;
  margin-top: -30px;
}

#header {
  transition: all 0.3s;
  background: steelblue;
  text-transform: uppercase;
  text-align: center;
  margin: 0;
  color: white;
}

#colorDisplay {
  font-size: 200%;
}
.square {
  width: 30%;
  background: blue;
  padding-bottom: 30%;
  float: left;
  margin: 1.66%;
  border-radius: 10%;
  transition: background 0.8s;
  -webkit-transition: background 0.8s;
  -moz-transition: background 0.8s;
}

#container {
  margin: 20px auto;
  max-width: 600px;
}
#message {
  color: black;
  display: inline-block;
  width: 20%;
}

#messageBox {
}
.selected {
  background-color: steelblue;
  color: white;
}

button {
  border: none;
  background-color: white;
  font-family: "Montserrat", "Avenir";
  text-transform: uppercase;
  height: 100%;
  font-weight: 700;
  letter-spacing: 1px;
  color: steelblue;
  transition: all 0.3s;
  outline: none;
}

button:hover {
  color: white;
  background-color: steelblue;
}
</style>
