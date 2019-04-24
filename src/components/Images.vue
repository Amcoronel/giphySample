<template>
 <div>

   <form v-on:submit.prevent="onSubmit" class="form"> 
     <input v-model="q" type="text" class="formInput" v-validate="'min:5'" name="input"/>
     <button type="submit" class="formButton">Enviar</button>
  </form>

  <div>
    <ul>
      <li v-for="(image, index) in images" :key="index">
        <a v-bind:href="image.url"><img  v-bind:src="image.url" /></a>
      </li>
    </ul>
  </div>

 </div>
</template>

<script>
import Axios from 'axios';
export default {
  name: 'Images',
  props: {
    msg: String
  }, 
  data(){
    return{
      q:"",
      images:[],
    }
  },
  methods:{
      onSubmit(){
          this.$validator.validate().then(valid =>{
            if(valid){
                this.images = [];
                Axios.get('http://api.giphy.com/v1/gifs/search?q=' + this.q + '&api_key=GWRBBqLx9s4Q2gSMM9ykF0YK8DiudbmY&limit=5').
                then((response)=>{
                  const images = response.data.data;
                  for(const image in images){
                    this.images.push({ url: images[image].images.original.url});
                  }
                });
            }
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
.form{
    background-color: white;
    width: 500px;
    margin:auto;
    box-shadow: 5px 4px 4px 0px rgba(132,131,131,0.75);
    padding:1rem 0;
    border-radius: 5px;
    margin-top:1rem;

    
}
.formInput{
    height: 30px;
    width: 80%;
    border-radius: 2px;
    margin-bottom: 1rem;
    padding:0.5rem;
    background-color: white;
}
.formButton{
    background-color: #42b983;
    color:white;
    width:40%;
    height: 40px;
    border-radius: 2px;
}
.formButton:hover{
    background-color: #1b5a3e;
}
</style>
