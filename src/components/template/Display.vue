<template>
  <div class="display">{{value}}</div>
</template>

<script>
export default {
    name: 'Display',
    props:{
        value: {}
    }
}
</script>

<style>
    @font-face {
        font-family: "Orbitron";
        src: url("../../assets/fonts/Orbitron-Regular.ttf");
    }

    .display{
        width: 100%;
        height: 40px;
        background-color: rgb(245, 245, 245);
        margin: 4px 2px;
        border-radius: 2px;
        padding: 6px;
        box-sizing: border-box;
        font-size: 20pxx;
        font-family: "Orbitron", sans-serif;

        display: flex;
        justify-content:flex-end;


        /*cross axis*/
        align-items: center;
    }

</style>