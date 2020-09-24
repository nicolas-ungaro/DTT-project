<template>
    <div id="search">
        <div class="input-div">
            <input placeholder="Search by title..." class="input" 
            v-model="searchInput" v-on:click=clearInput type="text">
            <img class="search-btn" src="../imgs/search-13-32.png" alt="" v-on:click=searchShow />
        </div>
        <!-- Toggle Button -->
        <div class="d-flex justify-content-center switch">
            <p>Movies</p>
            <div v-on:click=switchToggle class="toggle-button" id="toggle-btn">
                <div id="switcher" class="inner-circle"></div>
            </div>
            <p>TV</p>
        </div>
    </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';

@Component
export default class Search extends Vue {

    searchInput: string = "";
    type: string;

    clearInput() {
        this.searchInput = "";
    }

    searchShow() {
        this.$emit('send-search-input', this.searchInput, this.type);
    }

    switchToggle() {
        const switcher: HTMLElement = document.getElementById("switcher");
        const toggleBtn: HTMLElement = document.getElementById("toggle-btn");

        if (switcher.classList.contains("series")) {
            switcher.classList.remove("series");
            toggleBtn.classList.remove("diff-bg");
            this.type = "movies";
        }
        else {
            switcher.classList.add("series");
            toggleBtn.classList.add("diff-bg");
            this.type = "series";
        }

        this.$emit('send-type-toggle', this.type);
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@media(max-width:500px){
    .img_poster{
        width:130px;
    }
}
.loading-img{
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
}
.movie img{
    cursor: pointer;
    border-radius:10px;
    margin-bottom: 10px;
    margin-top: 10px;
    transform: scale(0.9);
    transition: all 0.3s ease;
}

.movie img:hover{
    transform: scale(1);
}

.toggle-button{
    height:24px;
    width:50px;
    background: rgb(255, 255, 255);
    border-radius:30px;
    margin-left:30px;
    margin-right:30px;
    padding:1.1px 1.2px;
    cursor: pointer;
    margin-top: 5px;
    transition: all 0.3s ease;
}

.diff-bg{
   background-color:rgb(255, 0, 0) ;

}
.inner-circle{
    background:rgb(255, 0, 0);
    height:22px;
    width:22px;
    border-radius:50%;
    transition:all 0.3s ease;
}
.series{
    margin-left:25.2px;
    background: white;
}
.switch{
    margin-top: 50px;
    color: rgb(218, 218, 218);
    font-size: 20px;
}
.movie{
    color:white;
    padding:20px;
}
.movie-div{
    transition: all 0.3s ease;
}
.input{
    background: transparent;
    color: white;
    border:none;
    border-bottom: 0.2px solid white;
    margin-top: 40px;
}
.input:focus{
    outline: none;
}
.input-div{
    margin-top: 40px;
}
.search-btn{
    cursor: pointer;
    width:24px;
}
.title-recommend{
    padding: 20px;
    display: flex;
}
.title-recommend h3{
    font-size: 30px;
}

label{
    font-size: 20px;
}
#sort{
    height:30px;
    margin-top: 3px;
    border-radius:5px;
}

#sort option{
    border-radius:5px;
}
</style>