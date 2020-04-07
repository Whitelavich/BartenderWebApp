<template>

<v-row class="px-2">

  <v-row v-if="drinks.length > 0"  >
    <v-col  v-for="(x,idx) of drinks" cols="6" md="3" lg="2"  :key="idx">
      <v-card elevation="24"  shaped width="100%"  :color="backgroundColor? backgroundColor: null"  >
        <v-row justify="center">

          <v-card-title> {{x.strDrink}}</v-card-title>
        </v-row>
        <v-row justify="center" class="pb-3">
          <img :src="x.strDrinkThumb" @click="x.isOpen = !x.isOpen" class="pb-2" @click.middle="openMenu" style="border-radius: 25%" width="80%">
        </v-row>
        <v-row v-if="x.isOpen">
          <v-col cols="6">
            <v-list class="shrink">
              <v-list-item v-for="(y,idx) of x.measures"  :key="idx">
                {{y}}
              </v-list-item>
            </v-list>
          </v-col>
          <v-col cols="6">
            <v-list>
              <v-list>
                <v-list-item v-for="(y,idx) of x.ingredients" :key="idx">
                  {{y}}
                </v-list-item>
              </v-list>
            </v-list>
          </v-col>
        </v-row>




      </v-card>
    </v-col>

  </v-row>
<v-row>

  <v-bottom-navigation fixed>


    <v-btn @click="getRandomDrinks" icon> Random</v-btn>
    <v-btn @click="getPopularDrinks" icon >Popular</v-btn>



    <v-select :items="ingredients" @input="filterCategory" class="shrink"></v-select>
    <v-text-field class="px-2 shrink "    label="Search" @input="searchByName"/>

  </v-bottom-navigation>
</v-row>


</v-row>




</template>

<script>
  import axios from 'axios';
  export default {
    name: 'drinks',
    props:{
      backgroundColor:{
        type: String,
      }
    },
    data: () =>{
      return{
        drinks: [],
        ingredients: [],
      }
    },
    mounted(){
      this.getRandomDrinks();
      this.getCategories();
      },
    methods:{

      async getRandomDrinks(){
        this.drinks = [];
for(let i = 0;i<5;i++){
  let url = 'https://www.thecocktaildb.com/api/json/v2/9973533/randomselection.php';
  let response = await axios.get(url);
  // console.log('response', response.data);
  this.cleanDrinks(response.data.drinks);
}
      },
      async getPopularDrinks(){
        this.drinks = [];
        let url = 'https://www.thecocktaildb.com/api/json/v2/9973533/popular.php';
        let response = await axios.get(url);
        // console.log('response', response.data);
        this.cleanDrinks(response.data.drinks);
      },
      async getCategories(){

        let url = 'https://www.thecocktaildb.com/api/json/v2/9973533/list.php?c=list';
        let response = await axios.get(url);
        this.ingredients = [];
        console.log('ing', response.data);
        for(let x of response.data.drinks){
          this.ingredients.push(x.strCategory);
        }
        console.log('After', this.ingredients)


      },
      async filterCategory(selection){
        this.drinks = [];
        let url = 'https://www.thecocktaildb.com/api/json/v2/9973533/filter.php?c=' + selection;
        let response = await axios.get(url);
        console.log('WOW',response);
        this.cleanDrinks(response.data.drinks);
      },
      async searchByName(value){
        if(value && value.length > 2){
          let url = 'https://www.thecocktaildb.com/api/json/v2/9973533/search.php?s=' + value;
          let response = await axios.get(url);
          if(response.data.drinks && response.data.drinks.length>0){
            this.drinks = [];
            this.cleanDrinks(response.data.drinks);
          }else{
            this.$swal({
              type: 'error',
              icon: 'error',
              titleText: 'No Results Found',
              confirmButtonText: 'Please Try Again',
              confirmButtonColor: 'green',
              toast: true,
              position: 'top' ,
              timer: 1500,
            })
          }

        }
      },
      cleanDrinks(drinksArray){
        for(let x of drinksArray){
          // console.log('x',x);
          x.isOpen = false;
          x.ingredients = [];
          x.measures = [];
          for(let i = 1; i<16;i++){

            let temp = 'if(x.strIngredient'+ i +'){'+
              'x.ingredients.push(x.strIngredient' + i + ')}';
           eval(temp);
          }
          for(let i = 1; i<16;i++){
            let temp ='if(x.strMeasure' + i + '){'+
              'x.measures.push(x.strMeasure' + i + ')}';
            eval(temp);
          }
         this.drinks.push(x);
        }

      },
    }
  }
</script>

<style scoped>

</style>
