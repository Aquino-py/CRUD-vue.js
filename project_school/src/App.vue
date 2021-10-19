<template>
  <div id="app">
    <Nav />
    <div class="marginPricipal">
      <router-view :key="$route.fullPath"></router-view>
    </div>
  </div>
</template>

<script>

import Nav from './components/_nav/Nav'

export default {
  name: 'App',
  components: {
    Nav
  }
}
</script>

<style>
#app {
  width: 60%;
}
@import url("https://fonts.googleapis.com/css?family=Montserrat:400,700");

body{
  background-color: #eee;
  font-family: "Montserrat", sans-serif;
  display: grid;
  justify-items: center;
}
body, html {
  margin: 0;
  height: 100%;
}

table {
  margin: 0px;
  padding: 0px;
  list-style-type: none;
  width: 100%;
}
table tr td{
  padding: 20px;
  font-size: 1.3em;
  background-color: #e0edf4;
  margin-bottom: 2px;
  color: #3e5252;
}
table thead th{
  background-color: rgb(184, 208, 216) !important;
  font-size: 1.2em;
  padding: 10px 0px;
  text-align: center !important;
}
.colPequeno {
  width: 5%;
  text-align: right;
  background-color: rgb(125, 217, 245);
  font-weight: bold;
}
.btn-danger {
  font-size: 0.8em !important;
  background-color: #fa4430;
}
.btn {
  border: 1px solid #000;
  cursor: pointer;
  padding: 10px 15px;
  color: white;
  font-size: 1.0em;
  font-weight: bold;
  border-radius: 5px;
  border-bottom: 3px solid black;
  position: static;
}
.btn:hover {
  text-shadow: 1px 1px 1px black;
  margin-top: 3px;
  border-bottom: 1px solid black;
}
input {
  width: calc(100%-150px);
  border: 0;
  padding: 20px;
  font-size: 1.3em;
  color: #687f7f;
  display: inline;
}
.btn-input {
  width: 150px;
  border: 0px;
  padding: 20px;
  font-size: 1.3em;
  background-color: rgb(116, 115, 155);
}
.btn-input:hover {
  padding: 20px;
  margin: 0px;
  border: 0px;
}
</style>
