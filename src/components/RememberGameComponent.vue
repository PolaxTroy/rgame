<template>
  <div class="control_elements">
    <div>
      <button class="button"
        @click="start"
      >
        Start
      </button>
    </div>
    
    <div>
      <button class="button"
        @click="timerPlus"
      >
        +
      </button>
      Timer: <b>{{ timerInit }}</b>
      <button class="button"
        @click="timerMinus"
      >
        -
      </button>
      <button class="button"
        @click="sizePlus"
      >
        +
      </button>
      Change grid
      <button class="button"
        @click="sizeMinus"
      >
        -
      </button>
    |
      <button class="button"
        @click="prizePlus"
      >
        +
      </button>
      Items: <b>{{prizeCount}}</b>
      <button class="button"
        @click="prizeMinus"
      >
        -
      </button>
    </div>
  </div>
  <div>
    <div
      v-for="x in size"
      :key="x"
      class="game_row"
    >
      <div
        v-for="y in size"
        :key="y"
      >
        <div
          class="square"
          v-bind:class="{ square_won: isWon(x-1, y-1), square_lost: isLost(x-1, y-1) }"
          @click="flipSquare(x-1,y-1)"
        ></div>
      </div>
    </div>
  </div>
  <div
    v-if="showToRemember"
  >
    {{ timer }}
  </div>
  <div
    v-if="victory"
  >
    <b>Victory!</b>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'RememberGameComponent',
  data: () => ({
      size: 4,
      prizeCount: 4,
      timer: 5,
      timerInit: 5,
      squares: [false],
      prizes: [false],
      showToRemember: false,
      victory: false,
  }),
  created() {
    this.fillData();
  },
  methods: {
    fillData() {
      this.victory = false;
      this.timer = this.timerInit;
      let i;
      for (i=0; i<=this.size*this.size; i++)
      {
        this.squares[i] = false;
      }
      this.prizes = new Array(this.squares.length);
      let t=0;
      while(t<this.prizeCount)
      {
        t=0;
        this.prizes[Math.floor(Math.random() * this.prizes.length-1)] = true;
        for (i=0; i<this.prizes.length; i++) if (this.prizes[i] === true) t++;
      }
    },
    isWon(x: number, y: number) {
      if (this.showToRemember && this.prizes[(this.size*y)+x]) return true;
      if (this.squares[(this.size*y)+x] && this.prizes[(this.size*y)+x]) return true;
      return false;
    },
    isLost(x: number, y: number) {
      if (this.squares[(this.size*y)+x] && !this.prizes[(this.size*y)+x]) return true;
      return false;
    },
    sizePlus() { if (this.size < 10) this.size++; this.fillData(); },
    sizeMinus() { if (this.size > 1) this.size--; this.fillData(); },
    timerPlus() { if (this.timer < 10) this.timerInit++; this.fillData(); },
    timerMinus() { if (this.timer > 1) this.timerInit--; this.fillData(); },
    prizePlus() { if (this.prizeCount < this.size*this.size) this.prizeCount++; this.fillData(); },
    prizeMinus() { if (this.prizeCount > 1) this.prizeCount--; this.fillData(); },
    flipSquare(x: number, y: number) {
      if (this.victory) return;
      this.squares[(this.size*y)+x] = true;
      let i;
      for (i=0;i<this.prizes.length; i++) {
        if (this.prizes[i] == true && this.squares[i] == false) return;
      }
      this.victory = true;
      return;
    },
    start() {
      this.fillData();
      this.showToRemember = true;
      const interval = setInterval(() => {
        if (this.timer === 0) {
          clearInterval(interval);           
          this.showToRemember = false;
        } else {
          this.timer--
        }             
      }, 1000)
    },
  },
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.game_row {
  width: auto;
  display: inline-block;
}
.game_item {
  width: 33%;
}
.square {
  height: 50px;
  width: 50px;
  background-color: #555;
  margin: 5px;
}
.square_won {
  background-color: greenyellow;
}
.square_lost {
  background-color: darkred;
}
.control_elements {
  width: 100%;
  margin: 10px;
  text-align: center;
  display: inline-block;
}
.button {
  margin: 10px;
}

</style>
