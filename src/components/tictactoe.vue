<template>
  <div id="app">
    <div class="status">
      {{ status + getFullCurrentPlayer(currentPlayer, true) }}
    </div>
    
    <br>
    
    <button @click="reset()">Reset</button>
    
    <div class='body-container'>
      <div class="squares-container">
        <button
          v-for="square in squares"
          :key="square.id"
          class="square"
          style="width: 40px; height: 40px"
          @click="check({ square })"
        >
          {{
            square.player === 1 && square.checked
              ? "X"
              : square.player === 2 && square.checked
              ? "0"
              : ""
          }}
        </button>
      </div>
    </div>
  </div>
</template>

<script>
const SQUARES_MODEL = [
  {
    id: 1,
    checked: false,
    player: 0,
    squarePosition: ["line1", "column1", "diagonal1"],
  },
  { id: 2, checked: false, player: 0, squarePosition: ["line1", "column2"] },
  {
    id: 3,
    checked: false,
    player: 0,
    squarePosition: ["line1", "column3", "diagonal2"],
  },
  { id: 4, checked: false, player: 0, squarePosition: ["line2", "column1"] },
  {
    id: 5,
    checked: false,
    player: 0,
    squarePosition: ["line2", "column2", "diagonal1", "diagonal2"],
  },
  { id: 6, checked: false, player: 0, squarePosition: ["line2", "column3"] },
  {
    id: 7,
    checked: false,
    player: 0,
    squarePosition: ["line3", "column1", "diagonal2"],
  },
  { id: 8, checked: false, player: 0, squarePosition: ["line3", "column2"] },
  {
    id: 9,
    checked: false,
    player: 0,
    squarePosition: ["line3", "column3", "diagonal1"],
  },
];

const PLAYER1 = {
  id: 1,
  name: "Daniel",
  line1: 0,
  line2: 0,
  line3: 0,
  column1: 0,
  column2: 0,
  column3: 0,
  diagonal1: 0,
  diagonal2: 0,
};

const PLAYER2 = {
  id: 2,
  name: "Mr. Robot",
  line1: 0,
  line2: 0,
  line3: 0,
  column1: 0,
  column2: 0,
  column3: 0,
  diagonal1: 0,
  diagonal2: 0,
};

export default {
  name: "App",
  data: () => ({
    gamers: [{ ...PLAYER1 }, { ...PLAYER2 }],
    currentPlayer: 1,
    squares: [...SQUARES_MODEL],
    status: "Next player: ",
  }),
  methods: {
    reset() {
      this.currentPlayer = 1;
      this.squares = [...SQUARES_MODEL];
      this.gamers = [{ ...PLAYER1 }, { ...PLAYER2 }];
    },
    check({ square, player }) {
      this.squares = this.squares.map((s) => {
        if (s.id === square.id) {
          return {
            ...s,
            checked: !s.checked,
            player: this.currentPlayer,
          };
        }

        return s;
      });

      this.checkResult(square);

      this.currentPlayer = this.currentPlayer === 1 ? 2 : 1;
    },
    getFullCurrentPlayer(currentPlayer, onlyName = false) {
      if (this.gamers.length > 0) {
        const player = this.gamers.find((p) => p.id === currentPlayer);
        return onlyName ? player.name : player;
      }
    },

    checkResult(square) {
      square.squarePosition.forEach((sP) => {
        const localCurrentPlayer = this.getFullCurrentPlayer(
          this.currentPlayer
        );
        localCurrentPlayer[sP] += 1;

        if (localCurrentPlayer[sP] == 3) {
          alert(`Jogador ${localCurrentPlayer.name} venceu!`);
          return;
        }
      });
    },
  },
};
</script>

<style>
.body-container {
  width: 100%;
  display: flex;
  justify-content: center;
  align-content: center;
  margin-top: 1em;
}
.squares-container {
  width: 150px;
  display: flex;
  flex-wrap: wrap;
}
</style>
