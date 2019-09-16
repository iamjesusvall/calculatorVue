<template>
  <div class="calculator">
    <div class="display">{{ display }}</div>
    <div @click="clear" class="btn">C</div>
    <div @click="sign" class="btn">+/-</div>
    <div @click="percent" class="btn">%</div>
    <div @click="divide" class="btn operator">&divide;</div>
    <div @click="append('7')" class="btn">7</div>
    <div @click="append('8')" class="btn">8</div>
    <div @click="append('9')" class="btn">9</div>
    <div @click="times" class="btn operator">&times;</div>
    <div @click="append('4')" class="btn">4</div>
    <div @click="append('5')" class="btn">5</div>
    <div @click="append('6')" class="btn">6</div>
    <div @click="minus" class="btn operator">-</div>
    <div @click="append('1')" class="btn">1</div>
    <div @click="append('2')" class="btn">2</div>
    <div @click="append('3')" class="btn">3</div>
    <div @click="add" class="btn operator">+</div>
    <div @click="append('0')" class="btn zero">0</div>
    <div @click="dot" class="btn">.</div>
    <div @click="equal" class="btn operator">=</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      previous: null,
      current : '',
      operator : null,
      operatorClicked : false
    }
  },
  methods: {
    clear() {
      this.current = ''
    },
    sign() {
      this.current = this.current.charAt(0) === '-' ? this.current.slice(1) : `-${this.current}`
    },
    percent() {
      this.current = `${ parseFloat( this.current ) / 100 }`
    },
    append(number) {
      if ( this.operatorClicked ) {
        this.current = ''
        this.operatorClicked = false
      }
      this.current = `${ this.current }${ number }`
    },
    dot() {
      if ( this.current.indexOf('.') === -1 ) {
        this.append('.')
      }
    },
    setPrevious() {
      this.previous = this.current
      this.operatorClicked = true
    },
    divide() {
      this.operator = (a, b) => a / b
      this.setPrevious() 
    },
    times() {
      this.operator = (a, b) => a * b
      this.setPrevious()
    },
    minus() {
      this.operator = (a, b) => a - b
      this.setPrevious()
    },
    add() {
      this.operator = (a, b) => a + b
      this.setPrevious()
    },
    equal() {
      if ( this.previous ) {
        this.current = `${this.operator(
          parseFloat( this.previous ),
          parseFloat( this.current )
          )}`
        this.previous = null
      }
    }
  },
  computed: {
    display() {
      if (this.current > 12) {
        return this.current.slice(0, 12)
        }
        return this.current || 0
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.calculator {
  margin: 0 auto;
  width: 400px;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(50px, auto);
  font-size: 48px;
}
.display {
  grid-column: 1 / span 4;
  background-color: #393e46;
  color: white;
  text-align: right;
  padding: 25px 40px;
}
.zero {
  grid-column: 1 / span 2;
  text-align: left;
  padding-left: 40px;
}
.btn {
  padding-top: 25px;
  padding-bottom: 25px;
  background-color: #eeeeee;
  border: 1px solid #232931;
  cursor: pointer;
}
.operator {
  background-color: #4ecca3;
}
.operator:hover {
  background-color: #2dcf99;
}
</style>
