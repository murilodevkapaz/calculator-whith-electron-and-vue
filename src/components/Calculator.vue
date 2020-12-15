<template>
  <div class="calculator">
    <Display :value="expression" />
    <Button label="C" class="dual" @onClick="cleanDisplay" />
    <Button label="(" @onClick="typeDisplay" />
    <Button label=")" @onClick="typeDisplay" />
    <Button label="1" @onClick="typeDisplay" />
    <Button label="2" @onClick="typeDisplay" />
    <Button label="3" @onClick="typeDisplay" />
    <Button label="+" @onClick="typeDisplay" />
    <Button label="4" @onClick="typeDisplay" />
    <Button label="5" @onClick="typeDisplay" />
    <Button label="6" @onClick="typeDisplay" />
    <Button label="-" @onClick="typeDisplay" />
    <Button label="7" @onClick="typeDisplay" />
    <Button label="8" @onClick="typeDisplay" />
    <Button label="9" @onClick="typeDisplay" />
    <Button label="*" @onClick="typeDisplay" />
    <Button label="0" @onClick="typeDisplay" />
    <Button label="." @onClick="typeDisplay" />
    <Button label="/" @onClick="typeDisplay" />
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
    cleanDisplay(){
        this.expression = 0;
    },
    typeDisplay(n) {
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
            const lastChar = this.expression[this.expression.length - 1];
            if (
              lastChar == "+" ||
              lastChar == "-" ||
              lastChar == "*" ||
              lastChar == "/"
            ) {
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