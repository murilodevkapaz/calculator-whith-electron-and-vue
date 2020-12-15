<template>
  <div class="calculator" @keyup.enter="typeDisplay">
    <Display :value="expression" />
    <Button label="C" class="dual" @onClick="cleanDisplay" />
    <Button label="(" @onClick="clickParentheses" />
    <Button label=")" @onClick="clickParentheses" />
    <Button label="1" @onClick="clickNumber" />
    <Button label="2" @onClick="clickNumber" />
    <Button label="3" @onClick="clickNumber" />
    <Button label="+" @onClick="clickOperation" />
    <Button label="4" @onClick="clickNumber" />
    <Button label="5" @onClick="clickNumber" />
    <Button label="6" @onClick="clickNumber" />
    <Button label="-" @onClick="clickOperation" />
    <Button label="7" @onClick="clickNumber" /> 
    <Button label="8" @onClick="clickNumber" />
    <Button label="9" @onClick="clickNumber" />
    <Button label="*" @onClick="clickOperation" />
    <Button label="0" @onClick="clickNumber" />
    <Button label="." @onClick="clickDot" />
    <Button label="/" @onClick="clickOperation" />
    <Button label="=" @onClick="typeDisplay" />
  </div>
</template>

<script>
import Button from "./template/Button";
import Display from "./template/Display";
export default {
  name: "Calculator",
  components: { Button, Display },
  data: function () {
    return {
      expression: "0",
    };
  },
  methods: {
    cleanDisplay() {
      this.expression = 0;
    },

    clickParentheses(n) {
      const lastChar = this.expression[this.expression.length - 1];

      if (n == "(") {
        if (!this.expression.includes("(")) {
          if (this.expression == "0") {
            this.expression = n;
          } else if (lastChar == ".") {
            this.expression = "0" + n;
          } else {
            const isOp = this.lastIsOperation();
            if (!isOp) {
              this.expression += "*" + n;
            }
            else{
              this.expression +=  n;
            }
          }
        }
      } else if (this.expression.includes("(")) {
        if (!this.expression.includes(")")) {
          const isOp = this.lastIsOperation();
          if (!isOp) {
            this.expression += n + "*";
          }
        }
      }
    },

    typeDisplay(n) {
      console.log(n);
      //verify if is a "."
      if (n === ".") {
        if (!this.expression.includes(".")) this.expression += n;
      } else {
        //verifty if is a number
        const val = parseFloat(n);
        if (!isNaN(val)) {
          if (this.expression == "0") this.expression = val.toString();
          else this.expression += val;
        } else {
          //whe be "="
          if (n === "=") {
            this.expression = eval(this.expression);
          } else {
            //when be operation
            var isOp = this.lastIsOperation();
            if (isOp) {
              this.expression = this.expression.substr(
                0,
                this.expression.length - 1
              );
              this.expression += n;
            } else {
              this.expression += n;
            }
          }
        }
      }
    },

    clickNumber(n) {
      const val = parseFloat(n);
      if (!isNaN(val)) {
        if (this.expression == "0") this.expression = val.toString();
        else this.expression += val;
      }
    },

    clickDot() {
      const isOp = this.lastIsOperation();
      if (!isOp) {
        if (!this.expression.includes(".")) this.expression += ".";
      }
    },

    clickOperation(n) {
      //when be operation
      var isOp = this.lastIsOperation();
      const lastChar = this.expression[this.expression.length - 1];
      if (isOp) {
        this.expression = this.expression.substr(0, this.expression.length - 1);
        this.expression += n;
      } else if (lastChar == ".") {
        this.expression += "0" + n;
      } else {
        this.expression += n;
      }
    },

    lastIsOperation() {
      const lastChar = this.expression[this.expression.length - 1];
      if (
        lastChar == "+" ||
        lastChar == "-" ||
        lastChar == "*" ||
        lastChar == "/"
      ) {
        return true;
      } else {
        return false;
      }
    },
  },
};
</script>

<style>
.calculator {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
}
</style>