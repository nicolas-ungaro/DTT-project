<template>
<div class="container">
    <nav class="navbar">
        <div class="nav-item"><a href="#app">Home</a></div>
        <div class="nav-item"><a href="">Categories</a></div>
        <div class="nav-item"><a href="">Random</a></div>
    </nav>
</div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import axios from "axios";

@Component
export default class Navbar extends Vue {

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.container{
    color: white;
}
.navbar{

}
.navbar a{
    text-decoration: none;
    color: white;
    text-transform: uppercase;
}
.nav-item{
    border-bottom:0.2px solid rgb(196, 194, 194);
    transition: all 0.5s ease;
    font-size: 20px;
}
.nav-item:hover{
     border-bottom:0.2px solid rgb(255, 0, 0);
     transform: scale(1.2);
}


</style>