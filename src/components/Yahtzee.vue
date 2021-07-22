<template>
  <div class="Yahtzee">
    <h1>Yahtzee</h1>
    <table>
    </table>

    <table>
      <tr>
        <th colspan="6">
          Thrown numbers
        </th>
      </tr>
      <tr>
        <th>1</th>
        <th>2</th>
        <th>3</th>
        <th>4</th>
        <th>5</th>
        <th>6</th>
      </tr>
      <tr>
        <td v-for="number in sortedDice" :key="number.value">{{number}}</td>
      </tr>
    </table>
    <br>

    <table>
      <tr>
        <th></th>
        <th>Score</th>
      </tr>
      <tr>
        <td>Three of a Kind</td>
        <td>{{scoreThreeOfAkind}}</td>
      </tr>
      <tr>
        <td>Four of a Kind</td>
        <td>{{scoreFourOfAkind}}</td>
      </tr>
      <tr>
        <td>Small Street</td>
        <td>{{scoreSmallStreet}}</td>
      </tr>
      <tr>
        <td>Large Street</td>
        <td>{{scoreLargeStreet}}</td>
      </tr>
      <tr>
        <td>Full House</td>
        <td>{{scoreFullHouse}}</td>
      </tr>
      <tr>
        <td>Chance</td>
        <td>{{total}}</td>
      </tr>
      <tr>
        <td>Yahtzee</td>
        <td>{{scoreYahtzee}}</td>
      </tr>
    </table>

    <button @click="throwDice">
      Throw dice!
    </button>
  </div>
</template>

<script>
export default {
  name: 'Yahtzee',

  data() {
    return {
      thrownNumbers: [],
    }
  },

  computed:{
    total() {
      return this.thrownNumbers.reduce((acc,d)=>acc+=d,0);
    },

    noDuplicates() {
      // Can return immediatly
      return [...new Set(this.thrownNumbers)]
      // let noDuplicates = [...new Set(this.thrownNumbers)]
      // console.log(noDuplicates);
      // return noDuplicates;
    },

    sortedDice() {
      // No need to check this if we loop over thrownNumbers
      // if(this.thrownNumbers.length == 0) return []

      const sortedNumbers = {
        1: 0,
        2: 0,
        3: 0,
        4: 0,
        5: 0,
        6: 0
      };

      // Looping over thrownNumbers is less intensive
      // for (const number in sortedNumbers) {
      //   sortedNumbers[number] = this.thrownNumbers.filter(die => die == number).length   
      // }

      for (const number of this.thrownNumbers) {
        sortedNumbers[number]++
      }

      return sortedNumbers;
    },

    scoreThreeOfAkind() {
      // A little less code, same result, just for inspiration
      const hasThree = Object.values(this.sortedDice).includes(3)
      return hasThree ? this.total : 0
      // for (const number in this.sortedDice) {
      //   if (this.sortedDice[number] >= 3) {
      //     return this.total;
      //   }
      // }
      // return 0;
    },

    scoreFourOfAkind() {
      for (const number in this.sortedDice) {
        if (this.sortedDice[number] >= 4) {
          return this.total;
        }
      }
      return 0;
    },

    scoreSmallStreet() {
      // Nice!
      let smallStreetProgress = 0;
      if (this.noDuplicates.length >= 4) {
        for (let i = 1; i < this.noDuplicates.length; i++) {
          if (this.noDuplicates[i] == (this.noDuplicates[i-1]+1)) {
            smallStreetProgress++;
          }
        }
        if(smallStreetProgress >= 3) {
          return 30;
        }
      }
      return 0;
    },

    scoreLargeStreet() {
      let largeStreetProgress = 1;
      if (this.noDuplicates.length == 5) {
        for (let i = 1; i < this.noDuplicates.length; i++) {
          if (this.noDuplicates[i] != (this.noDuplicates[i-1]+1)) {
            largeStreetProgress--;
          }
        }
        if(largeStreetProgress == 1) {
          return 40;
        }
      }
      return 0;
    },

    scoreFullHouse() {
      if (this.noDuplicates.length !== 2) return 0

      // A little less code, same result, just for inspiration
      const hasThree = Object.values(this.sortedDice).includes(3)
      const hasTwo = Object.values(this.sortedDice).includes(2)

      return hasTwo && hasThree ? 25 : 0

      // if (this.noDuplicates.length == 2) {
      //   for (const number in this.sortedDice) {
      //     if (this.sortedDice[number] >= 3) {
      //       for (const number2 in this.sortedDice) {
      //         if (this.sortedDice[number2] == 2) {
      //           return 25;
      //         }
      //       }
      //     }
      //   }
      // }
      // return 0;
    },

    scoreYahtzee() {
      for (const number in this.sortedDice) {
        if (this.sortedDice[number] == 5) {
          return 50;
        }
      }
      return 0;
    }
  },

  methods: {
    throwDice() {
      this.thrownNumbers = [];
      for (let i=1; i < 6; i++) {
        this.thrownNumbers.push(Math.ceil(Math.random()*6));
      }
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
table, th, td {
  padding: 10px;
  border: 1px solid black;
  border-collapse: collapse;
  margin-left: auto;
  margin-right: auto;
}
</style>
