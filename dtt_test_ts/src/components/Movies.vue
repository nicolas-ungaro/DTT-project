<template>
<div class="container">
    <!-- Input for search -->
    <div class="input-div">
        <input placeholder="Search by title..." class="input" 
        v-model ="search" v-on:click="search = '' " type="text">
        <img class="search-btn" src="../imgs/search-13-32.png" alt="" v-on:click = getMoviesBySearch ><img >
    </div>
    <!-- Toggle Button -->
    <div class="d-flex justify-content-center switch">
        <p>Movies</p>
        <div v-on:click="switchToggle();getMoviesBySearch()" class="toggle-button" id="toggle-btn">
            <div id = "switcher" class="inner-circle"></div>
        </div>
        <p>TV</p>
    </div>
      <!-- Loading gif -->
    <div class="loading-img" v-if="movies.length == 0">
        <img src="../imgs/ajax-loader.gif" alt="">
    </div>
    <div class="movie row">

    <!--After getting API response  -->
    <div class="title-recommend" v-if="recommended">
        <h3>Recommended for you</h3>
    </div>
    <div class="title-recommend" v-if="!recommended">
        <label for="sort">Sort by:</label>

        <select class="ml-2" name="sort" id="sort"  v-model="sort" v-on:change="">
            <option value="year">Year</option>
        </select>
    </div>

     <div v-if="searchSeries" class="row">
        <div class="col-lg-3 col-6"  v-for="movie in movies" v-if="movie.poster_path">
            <img class="img_poster" :src=" urlImg + movie.poster_path" alt="">
            <h5 >{{movie.original_name}}</h5>
            <h6>{{(new Date(movie.first_air_date).getFullYear())}}</h6>
        </div>
    </div>
    <div v-if="!searchSeries" class="row">
        <div class="col-lg-3 col-6 movie-div" v-for="movie in movies"  v-if="movie.poster_path">
            <img class="img_poster" :src=" urlImg + movie.poster_path" alt="">
            <h5 >{{movie.original_title}}</h5>
            <h6>{{(new Date(movie.release_date).getFullYear())}}</h6>
        </div>
    </div>
  </div>
</div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import axios from "axios";

@Component
export default class Movies extends Vue {

urlImg: String = "https://image.tmdb.org/t/p/w200/"

//Create array to store the movie data
movies: Array<object> = []
search: String = ""
sort: String = ""
searchSeries: boolean = false
recommended: boolean = true

// Function to call API
getMoviesBySearch(){

    if(!this.search){
        if(!this.searchSeries){
        axios.get("https://api.themoviedb.org/3/trending/movie/day?api_key=0567971fd9aa85a3b7dcd6d28eeabd21")
        .then(response => this.movies = response.data.results)
        .catch(err=> console.log(err))
        this.recommended = true;
    }
        if(this.searchSeries){
        axios.get("https://api.themoviedb.org/3/trending/tv/day?api_key=0567971fd9aa85a3b7dcd6d28eeabd21")
        .then(response => this.movies = response.data.results)
        .catch(err=> console.log(err));
        this.recommended = true;
        }

    }


    if(this.search){

        if(!this.searchSeries){
        axios.get("https://api.themoviedb.org/3/search/movie?query="+ this.search + " &api_key=0567971fd9aa85a3b7dcd6d28eeabd21&language=en-US&page=1&include_adult=false")
        .then(response => this.movies = response.data.results)
        .catch(err=> console.log(err));
        this.recommended = false;
    
    }
     else{
        axios.get("https://api.themoviedb.org/3/search/tv?query="+ this.search + " &api_key=0567971fd9aa85a3b7dcd6d28eeabd21&language=en-US&page=1&include_adult=false")
        .then(response => this.movies = response.data.results)
        .catch(err=> console.log(err))
        this.recommended = false;
    }
    }
}

// Function to switch toggle (series ans movies)
switchToggle(){
    const element: HTMLElement = document.getElementById("switcher");
    const toggleBtn: HTMLElement = document.getElementById("toggle-btn")
    if(element.classList.contains("series")){
        element.classList.remove("series");
        this.searchSeries = !this.searchSeries;
        toggleBtn.classList.remove("diff-bg")

    }

    else{
        element.classList.add("series");
        this.searchSeries = !this.searchSeries;
        toggleBtn.classList.add("diff-bg")
    }
    
}

// Funtion to sort by year



// Calling function to get the recommended movies
created(){
   this.getMoviesBySearch();
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