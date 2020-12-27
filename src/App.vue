<template>
  <div id="app">
    <div style="background-color: rgba(0,0,0,0.5);height: 100vh">
    <nav class="navbar navbar-dark sticky-top">
      <div class="text-center navbar-brand">
        <!-- <span style="color : #1c85f5">Q</span>    
        <span style="color : #ff009d">U</span>
        <span style="color : #cccebf">I</span>
        <span style="color : #13e02f">Z</span>
        <p style="width:20px; display : inline-block"></p>
        <span style="color : #e2082c">L</span> -->
        <!-- <span style="color : #fadd2a">o</span> -->
        <!-- <span style="color : #f56e00">A</span>
        <span style="color : #00f5a3">B</span> -->
        <span >
          <span style="bottom : 70px">
<!-- style="color: white;font-weight: bolder;-webkit-text-fill-color: transparent; -webkit-text-stroke-width: 1px;" -->
          <img src="./assets/brandlogo.png" alt="" width="50px" >
          </span>
          QUIZ SHARKOR
        </span>
      </div>
    </nav>

    <Setup v-if="!setupComplete" @generator="generator"/>
        <QuestionBox 
          v-if="questions.length"
          :currentQuestion = "questions[index]"
          :next="next"
          :stringing = "stringing"
          :indexing = "indexing"
        />
    </div>
  </div>
</template>

<script>
  import QuestionBox from './components/QuestionBox.vue'
  import Setup from './components/Setup.vue'

  export default {
    name: 'App',
    components: {
      QuestionBox,
      Setup
    },
    // props : {
    //   getURLParams : Funtion
    // },
    data(){
      return{
        questions : [],
        index : 0,
        stringing: '',
        setupComplete : false
      }
    },
    computed : {
      indexing(){
        return this.index;
      }
    },
    methods : {
      generator(URLparameters){
        // fetch('https://opentdb.com/api.php?amount=10&category=18&type=multiple',{
// Math.floor(Math.random()*(Math.floor(32)-Math.ceil(9)+1))
        // if(URLparameters[1] == null){
        //   URLparameters[1] = Math.floor(Math.random() *(32 - 9 + 1))+9;
        // }
        var link = 
        `https://opentdb.com/api.php?amount=${URLparameters[0]}&category=${URLparameters[1]}&difficulty=${URLparameters[2]}&type=${URLparameters[3]}`
          console.log(link);
          // console.log(URLparameters[1]);
        fetch(
          link,
          {method: 'get'}
        )
        .then((response) => {
          return response.json()
        })
        .then((jsonData) => {
          this.questions = jsonData.results 
        })

        this.setupComplete = true;
      },
      next(){
        if(this.index < this.questions.length-1){
          this.index++;
        }else if(this.index == this.questions.length-1){
           var vanish = document.getElementById("vanish");
           var child = document.getElementsByClassName("card");
           var show = document.getElementById('show');
          // for(var i=0; i < vanish.length-1; i++){
            vanish.removeChild(child[0]);
            // this.questions.pop(this.questions[i])
          // }
          show.style.display = 'block';
        }
      }
    }
  }
</script>

<style>
  *{
    overflow-x: hidden;
  }
  *::selection{
      background-color: #404549;
    }
  #app{
    background-image: url('./assets/blocks.jpg');
    /* background-clip: ; */
    background-position: center center;
    background-size: cover;
    background-attachment: fixed; 
    background-repeat: no-repeat;
    background-repeat: center center; 
    height: 100vh;
    overflow-x: hidden;
    /* overflow-y: hidden; */
  }

  .navbar div.navbar-brand{
    font-family: calibri;
    font-size: 50px;
    text-align: center;
    margin: auto;
    padding: 0px;
  }
  .navbar{
    padding: 0px;
    box-shadow: 2px 2px 8px rgb(62, 62, 62);
    background-color: rgba(22, 125, 243, .7);
  }
</style>