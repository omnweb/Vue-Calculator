<template>
  <div id="app2">
        <h1>Calculadora</h1>
        <Calculator />
    </div>
</template>

<script>
//Imporando para o app a calculadora
import Calculator from './main/Calculator'
export default {
//referenciando a calculadora 
components:{Calculator}
}
</script>

<style>
@font-face {
    font-family: "RobotoMono";
    src: url("./fonts/RobotoMono-Thin.ttf");
}
* {
    font-family: "RobotoMono", monospace;
}
body {
    margin: 0;
}
#app2 {
    display: flex;
    flex-direction: column;
    height: 100vh;
    justify-content: center;
    align-items: center;
    text-align: center;
    color: #fff;
    background: linear-gradient(to right, #c31432, #240b36); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
    }
</style>