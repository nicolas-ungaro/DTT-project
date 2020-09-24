<template>
<div class="container">
    <Search v-on:send-search-input="handleSearch" v-on:send-type-toggle="handleToggle"/>
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
import Search from './Search.vue';

@Component({
  name: 'Movies',
  components: {
    Search,
  }
})
export default class Movies extends Vue {
    apiKey: string = "0567971fd9aa85a3b7dcd6d28eeabd21";
    urlImg: String = "https://image.tmdb.org/t/p/w200/"

    //Create array to store the movie data
    movies: Array<object> = []
    search: string = ""
    sort: string = ""
    searchSeries: boolean = false
    recommended: boolean = true

    // Function to call API
    handleSearch(searchInput, type) {
        this.search = searchInput;
        this.searchSeries = type === "series";
        let apiUrl : string = this.getApiUrl(this.search, this.searchSeries);
        let query : string = this.search === "" ? "" : `query=${this.search}&`

        axios.get(`${apiUrl}?${query}api_key=${this.apiKey}`)
            .then(response => this.movies = response.data.results)
            .catch(err=> console.log(err))
        this.recommended = this.search === "";
    }

    getApiUrl(search : String, series: boolean) : string {
        let resource : string = series ? "tv" : "movie";
        let api : string = search === "" ? "trending" : "search";
        let resourceSubPath : string = search === "" ? "day" : "";

        return `https://api.themoviedb.org/3/${api}/${resource}/${resourceSubPath}`;
    }

    // Function to switch toggle (series ans movies)
    handleToggle(type) {
        this.handleSearch(this.search, type);
    }

    // Calling function to get the recommended movies
    created() {
        this.handleSearch(this.search, this.searchSeries ? "series" : "movies");
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