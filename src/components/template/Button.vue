<template>
  <button class="button" :class="{ dual }" @click="$emit('onClick', label)">
    {{ label }}
  </button>
</template>

<script>
export default {
  name: "Button",
  //   props: [
  //       "nameButton",
  //   ],
  props: {
    label: {},
    dual: { type: Boolean },
  },
  data: function () {
    return {
      //   label: {}
    };
  },
};
</script>

<style>
.button {
  width: 25%;
  height: 50px;

  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;

  border-bottom: 5px solid #6b6b71;
  box-sizing: border-box;
  border-right: 5px solid #4d4b4b;
  border-left: 5px solid #4d4b4b;
  border-top: 5px solid #857c7c;
  color: black;
  background-color: #bbaeae;
  font-weight: bold;
}
.dual {
  width: 50%;
}
</style>