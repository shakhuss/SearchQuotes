<template>
    <div class="container">
        <div class="top">
            <h1>Welcome To the Quotes</h1>
        
            <div class="searchin">
                <input type="text" v-model="search" placeholder="Search for Quote"/> <!-- The input box to search for quote -->
            </div> 
            <div class="button-select">
                <button v-for="(item, index) in filterButtons" :item="item" :key="index" @click="filter = item; active = index; page = 1" :class="{active: item == filter}">
                    {{ item | capitalize}}   <!-- Iterates through the filterButtons array and produces the buttons. Page is set to 1 every time a filter is selected -->
                </button>
            </div>
        </div>
        <div class="content">
            <div v-if="filter === 'All'">
                <thequote class="card" v-for="(item, index) in filteredQuotes" :key="index" :item="item"></thequote> <!-- Displays all quotes if button 'All' is selected-->
            </div>
            <div class="filterquo" v-for="(item, index) in filteredQuotes" :item="item" :key="index" > <!-- Iterates throught the Quotes   -->
                <thequote class="card" v-if="item.theme === filter" :key="index" :item="item"></thequote> <!-- Filters quotes based on selection of Movies or Games -->
            </div>
            
            <div class="button-nav">
                <button class="prevPages" v-if="page != 1" @click="page--"> Previous </button> <!-- Previous button is only shown when the current page is not 1  -->
                <button v-for="pageNumber in pages"  v-bind:key="pageNumber" @click="page = pageNumber" :class="{active:pageNumber == page}">{{pageNumber}}</button> <!-- Iterates through the pages array and produces buttons for each  -->
                <button class="nexPages" v-if="page < pages.length" @click="page++"  > Next </button> <!-- Next button is shown if there is more than 1 page  -->
            </div>
        </div>
    </div>

</template>


<script>

import TheQuote from './TheQuote.vue';
import axios from 'axios';

export default {
    name: 'SearchQuote',

    data() {
        return{
            search:'', // Empty string for the search
            thequotes: [],  // Array used for pushing the quotes from the get method
            filterButtons: ["All","movies", "games"], // Array for the filters
            filter: "All", //Default filter is set to 'All' showing quotes
            quotes: [], //Array for quotes
            pages:[], // Array for the number of pages calculated
            perPage: '15', //Default per page limit to 15
            page: "1" // Page is set to 1
        }
    },

    mounted(){
        axios.get('https://gist.githubusercontent.com/benchprep/dffc3bffa9704626aa8832a3b4de5b27/raw/quotes.json').then(response => (this.thequotes = response.data)) //Use of axios enable to fetch quotes from online
    },

    components: {
        'thequote': TheQuote    //Using the component to help display quotes
    },  
    
    methods:{
        setPages(){
            var numberOfPages = Math.ceil(this.quotes.length / this.perPage);   // Calculates the number of pages needed by dividing length of quotes array by 15
            this.pages = [];    // Sets the pages array to empty to prevent the accumulation of pages after each filter selection
            if(this.pages.length === 0){    
            for (var i = 1; i <= numberOfPages; i++){   // For loop to add each page to the pages array
                this.pages.push(i);
            }
            }
        },

        quotesPage(quotes) {
            let begin = ( this.page * this.perPage ) - this.perPage;    // Calculates the start point of the quotes selection for each page
            let end = ( this.page * this.perPage);  // Calculates the end point of the quotes selections for each page
            return quotes.slice(begin,end); // Uses the slice method to return the quotes on each designated page
        }


    },   
    
    
    computed: {

            filteredQuotes() {
                this.quotes = [];
                if(this.search.length != 0){
                    this.quotes = this.thequotes.filter(item => item.quote.toLowerCase().includes(this.search.toLowerCase())); // Takes the quote and search string and converts to lowercase in order to make a match
                    return this.quotesPage(this.quotes);    // Returns the quotes to get sliced
                }

                if(this.filter === 'games'){                        // Checks the filter selection and pushes the quotes matching the filter to the quotes array
                    for(var i = 0; i < this.thequotes.length; i++){
                        if(this.thequotes[i].theme === 'games'){
                            this.quotes.push(this.thequotes[i]);
                        }
                    }
                    return this.quotesPage(this.quotes);                    
                }
                if(this.filter === 'movies'){
                    for(var i = 0; i < this.thequotes.length; i++){
                        if(this.thequotes[i].theme === 'movies'){
                            this.quotes.push(this.thequotes[i]);
                        }
                    }
                    return this.quotesPage(this.quotes);                    
                }
                if(this.filter === 'All'){
                    for(var i = 0; i < this.thequotes.length; i++){
                        this.quotes.push(this.thequotes[i]);
                    }
                    return this.quotesPage(this.quotes);                    
                }
                


            }
        },
    
    watch:{
        quotes() {
            this.setPages();    //Method used to check for any changes in the quotes array and calls the setPages method
        }
    },

    filters: {
    capitalize: function (value) {  // Used to capitalize the first letter in the filters
        if (!value) return ''
        value = value.toString()
        return value.charAt(0).toUpperCase() + value.slice(1)
    }
}    




};



</script>

<style>

html,body
{
    width: 100%;
    height: 100%;
    margin: 0px;
    padding: 0px;
    overflow-x: hidden; 
}

.button-select button{
    border: 1px #333;
    color: rgb(82, 220, 255);
    border-radius: 6px;
    background-color: rgb(121, 121, 121);
    padding: 15px 32px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
    cursor: pointer;
    box-shadow: 0 8px 16px 0 rgba(0,0,0,0.2), 0 6px 20px 0 rgba(0,0,0,0.19);

}

.button-select button:active {
  background-color: #333;
  box-shadow: 0 5px #666;
  transform: translateY(4px);
}


.button-nav button{
    border: 1px;
    color: rgb(82, 220, 255);
    background: rgb(121, 121, 121);
    padding: 10px 25px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 14px;
    cursor: pointer;
    box-shadow: 0 8px 16px 0 rgba(0,0,0,0.2), 0 6px 20px 0 rgba(0,0,0,0.19);
       
}


.button-nav .active{
  background:#333;
}


h1, h2 {
  font-weight: bold;
  color: rgb(41, 174, 207);
  font-style: italic;
}

.top h1{
    color:rgb(82, 220, 255);
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

input[type=text] {
  border: 2px solid rgb(82, 220, 255);
  width: 60%;
  padding: 12px 20px;
  margin: 8px 0;
  box-sizing: border-box;
  border-radius: 4px;
}

 
.content {
  background-color: #ddd;
  padding: 10px;
  width: 100%;
  
}

.template{
    background-color: #ddd;
}

.top {
    overflow: hidden;
    background-color: #fff;
    width: 100%;
    height: 200px;
    padding: 10px; 
}

</style>