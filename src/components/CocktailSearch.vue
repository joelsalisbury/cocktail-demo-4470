<template>
  <div class="cocktail-wrap">
    <h1>{{header}}</h1>

    <input placeholder="Type your search here." class="searchString" type="text" v-model="searchString">
    <button @click="doSearch" class="searchButton">Search</button>

    <div class="searchResults" v-if="results.length">
        <ul v-if="results.length">
            <li @click="loadSingleDrink(drink.idDrink)" v-for="drink in results" :key="drink.idDrink">
                <img :src="drink.strDrinkThumb">
                {{drink.strDrink}}
            </li>
        </ul>
    </div>

    <div class="overlay" v-if="showSingleDrink">
        <div style="width:80%; padding:50px;">
            <h3>{{activeDrink.strDrink}}</h3>
            <img style="max-width:150px;" :src="activeDrink.strDrinkThumb"/>
            <h4>Ingredients:</h4>
            <ul>
                <li v-for="ing in activeDrink.ingredients" :key="ing.ing">
                    {{ing.amount}} - {{ing.ing}}
                </li>
            </ul>
            <h4>Instructions:</h4>
            <p class="directions">{{activeDrink.strInstructions}}</p>
            <button @click="showSingleDrink = false">Close</button>
        </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data:() => {
      return {
          header:"Welcome to CocktailSearch",
          searchString:"",
          results:[],
          showSingleDrink:false,
          activeDrink:{}
      }
  }, 
  methods: {
      doSearch: function (){
        console.log("In DoSearch()...");
        console.log(`Search string is: ${this.searchString}`);
        fetch(`https://www.thecocktaildb.com/api/json/v1/1/search.php?s=${this.searchString}`)
        .then(response => response.json())
        .then(data => {
            this.results = data.drinks;
        });
      },

      loadSingleDrink: function(drinkId) {
        console.log("In loadSingleDrink()...");
        console.log(`Drink ID to load is: ${drinkId}`);
        var that = this;
        //https://www.thecocktaildb.com/api/json/v1/1/lookup.php?i=11007
        fetch(`https://www.thecocktaildb.com/api/json/v1/1/lookup.php?i=${drinkId}`)
        .then(response => response.json())
        .then(data => {
            that.activeDrink = data.drinks[0];
            that.showSingleDrink = true;
            that.activeDrink.ingredients = [];

            for(var i=1; i<=15; i++) {
                if(that.activeDrink['strIngredient' +i] !== null) {
                    var ingred = that.activeDrink['strIngredient' + i];
                    var amount = that.activeDrink['strMeasure' + i];
                    
                    var ing = {
                        amount:amount,
                        ing:ingred
                    }
                    that.activeDrink.ingredients.push(ing)
                }
            }
            console.log(that.activeDrink);
        });
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}

a {
  color: #42b983;
}

.searchResults ul {
    list-style-type: none;
    margin:0px;
    padding:0px;
}

.searchResults li {
    margin:0px;
    padding:15px 0px;
    background-color:#eee;
    margin: 2px 0px;
    width:100%;
    text-align: center;
    list-style-type: none;
    cursor: pointer;
}

.searchResults li img {
    width:75px;
}

.searchResults li:hover {
    background-color:#42b983;
    color:#fff;
}

.searchString {
    width:80%;
    margin:0 auto;
    height:42px;
    font-size: 18px;
    padding:0px;
}

.searchButton {
    height:42px;
    background-color:#42b983;
    border:none;
    color:#eee;
    position:relative;
    left:-2px;
    top:-1px;
    cursor:pointer;
}

.overlay {
    position: fixed;
    top:0px;
    left:0px;
    right:0px;
    bottom:0px;
    background-color:rgba(0,0,0,0.75);
    display:flex;
    align-items:center;
    justify-content: center;
    color:#fff;
}

</style>
