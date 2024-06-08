  <template>
  <div id="app">
    <div class="calcul">
      <h1 class="titre">💻 Calculatrice 💻</h1>
      <input type="text" class="resultat" :value="result" readonly />
      <div class="buttons">
        <button class="numero" @click="ClickButton('7')">7</button>
        <button class="numero" @click="ClickButton('8')">8</button>
        <button class="numero" @click="ClickButton('9')">9</button>
        <button class="operations" @click="OperationClick('/')">/</button>

        <button class="numero" @click="ClickButton('4')">4</button>
        <button class="numero" @click="ClickButton('5')">5</button>
        <button class="numero" @click="ClickButton('6')">6</button>
        <button class="operations" @click="OperationClick('*')">*</button>

        <button class="numero" @click="ClickButton('1')">1</button>
        <button class="numero" @click="ClickButton('2')">2</button>
        <button class="numero" @click="ClickButton('3')">3</button>
        <button class="operations" @click="OperationClick('-')">-</button>

        <button class="numero" @click="ClickButton('0')">0</button>
        <button class="numero" @click="ClickButton('.')">.</button>
        <button class="numero" @click="ClickButton('00')">00</button>
        <button class="operations" @click="OperationClick('+')">+</button>

        <button class="operations" @click="ClickButton('(')">(</button>
        <button class="operations" @click="ClickButton(')')">)</button>
        <button class="operations" @click="OperationClick('^')">^</button>
        <button class="operations" @click="OperationClick('Math.sqrt(')">√</button>
        <button class="clear" @click="buttonClear">C</button>
        <button class="clear" @click="buttonEntry">CE</button>
        <button class="egal" @click="calculate()">=</button>
        <button class="operations" @click="ClickButton('Math.PI')">π</button>
      </div>
    </div>
  </div>  
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      result: '',
      calculated: false
    }
  },
  methods: {
    ClickButton(value) {
      if (this.calculated) {
        this.result = value;
        this.calculated = false;
      } else {
        
        if ((value === '(' || value === 'Math.PI') && /[0-9)]$/.test(this.result)) {
          this.result += '*';
        }

        if (value === 'Math.PI') {
          
          this.result += 'π';
        } else if (value === '**') {
          
          this.result += '^';
        } else {
          this.result += value;
        }
      }
    },
    buttonClear() {
      this.result = '';
      this.calculated = false;
    },
    buttonEntry() {
      if (this.result && this.result.length > 0) {
        this.result = this.result.slice(0, -1);
        if (this.result.length === 0) {
          this.buttonClear();
        }
      } else {
        this.buttonClear();
      }
    },
    OperationClick(operator) {
      if (operator === 'Math.sqrt(') {
        if (/[0-9)]$/.test(this.result)) {
          this.result += '*√(';
        } else {
          this.result += '√(';
        }
      } else if (operator === '**') {
      
        if (/[+*/-]$/.test(this.result)) {
          this.result = this.result.slice(0, -1) + '^';
        } else {
          this.result += '^';
        }
      } else {
        if (/[+*/-]$/.test(this.result)) {
          this.result = this.result.slice(0, -1) + operator;
        } else {
          this.result += operator;
        }
      }
      this.calculated = false;
    },
    calculate() {
  try {
    let expression = this.result;

    
    expression = expression.replace(/π/g, "Math.PI");
    expression = expression.replace(/\^/g, "**");

    expression = expression.replace(/\b0+([0-9])/g, '$1');
    expression = expression.replace(/√/g, "Math.sqrt");
    expression = expression.replace(/\^/g, "^");


    let openParens = (expression.match(/\(/g) || []).length;
    let closeParens = (expression.match(/\)/g) || []).length;
    while (openParens > closeParens) {
      expression += ')';
      closeParens++;
    }

 
    let evaluatedResult = eval(expression);
    if (evaluatedResult === Infinity || evaluatedResult === -Infinity) {
      throw new Error('Erreur de division par zéro');
    }
    this.result = Number.isFinite(evaluatedResult) ? evaluatedResult.toString() : 'Erreur';
    this.calculated = true;
  } catch (error) {
    this.result = 'Erreur: ' + error.message;
  }
}
    
  }
}

</script>

<style>
  @import './assets/style.css';
</style>