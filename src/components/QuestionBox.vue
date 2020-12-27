<template>
    <div class="container">
        <div class="row mt-3">
            <div class="col-md-10 offset-md-2 col-lg-10 offset-lg-1" id="vanish">
                <div class="card" >
                    <div class="card-body">
                        <div id="Qnum" class="text-center m-0">Question {{indexing+1}}</div>
                        <div style="text-align:center; font-size:20px">{{currentQuestion.question}}</div>
                        <hr class="my-3">
                        <div class="list-group d-flex">
                            <div class="list-group-item p-0">
                                <button class="btn listBtn" style="display: block"
                                v-for="(answer, index) in answers"
                                :key="index"
                                @click="selectedAnswer(index)"
                                >
                                    {{ answer }}
                                    <!-- <i class="fa fa-check bg-danger"></i> -->
                                </button>
                            </div>
                        </div>
                        <div class="d-flex justify-content-center">
                            <button id="congrats" class="btn btn-primary mt-3"
                            @click="submitAnswer"
                            :disabled="selectedIndex === null || answered">
                            Submit Answer
                            </button>
                        </div>

                        <hr class="my-4">
                        <div class="row offset-1">
                            <div class="col-xs-5 col-md-5">
                                {{stringing}}
                                Number of Corrects : <strong>{{numCorrect}}</strong>
                            </div>
                            <div class="col-xs-7 col-md-7">
                                Number of Questions Answered : <strong>{{numTotal }}</strong> 
                            </div>
                            <div class="col-xs-12 col-md-12">
                                Counter: <strong>{{ numCorrect }} / {{ numTotal }}</strong> 
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="row mt-3" id="show" style="display: none">
            <div class="col-md-10 offset-md-2 col-lg-10 offset-lg-1">
                <div class="jumbotron text-center p-0 pb-4">   
                    <div id="endQuiz">
                        END OF QUIZ 
                    </div>                  
                    <div class="py-3 my-4" style="border: 2px solid grey; border-radius: 10px;width: 90%; margin: auto;">
                        <div id="results">
                           <p style="margin: 0px;font-weight: bold;font-size: 25px;">Results</p> 
                        </div>
                        <div class="row">
                            <div class="col-sm-12">
                                <div class="row">
                                    <div class="col sm-12 col-md-12">
                                         <p class="info">Total number of questions answered : <strong>{{ numTotal }}</strong> </p>
                                    </div>
                                </div>
                            </div>
                            <div class="col-sm-12">
                                <div class="row">
                                    <div class="col sm-12 col-md-12">
                                         <p class="info">Number of questions answered correctly : <strong>{{ numCorrect }}</strong></p>     
                                    </div>
                                </div>
                            </div>
                            <div class="col-sm-12">
                                <div class="row">
                                    <div class="col sm-12 col-md-12">
                                         <p class="info">Number of questions answered incorrectly : <strong>{{ numTotal - numCorrect }}</strong></p>
                                    </div>
                                </div>
                            </div>
                            <div class="col-sm-12">
                                <div class="row">
                                    <div class="col sm-12 col-md-12">
                                         <p class="info">Percentage Score : <strong>{{ percent }}%</strong></p>
                                    </div>
                                </div>
                            </div>
                            <div class="col-sm-12">
                                <div class="row">
                                    <div class="col sm-12 col-md-12">
                                         <p class="info">Remarks : <strong>{{performance}}</strong></p>
                                        
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div>
                            <span class="lead" style="font-weight: bold;">You scored</span>
                            <div id="score"><strong>{{ numCorrect }} / {{ numTotal }}</strong></div>
                        </div>
                    </div>     
                    <button class="btn btn-block btn-primary mt-4" id="reload" @click="reloadPage">Take a new quiz</button>
                 </div>
            </div>
        </div>
    </div>
</template>


<script>
import _ from 'lodash'
export default {
    props:{
        currentQuestion : Object,
        next : Function,
        indexing : Number, 
        stringing : String,
    },
    data(){
        return{
            selectedIndex: null,
            answered : false,
            correctIndex : null,
            isCorrect : false,
            numCorrect : 0,
            numTotal : 0,
            percent : 0,
            performance : '',
            // timeVal : 15
        }
    },
    watch : {
        currentQuestion : {
            immediate : true,
            handler(){
                this.selectedIndex = null;
                this.answered = false;
                this.getCorrectIndex();
                // this.timer();
            }
        }     
    },
    computed:{
        answers(){
            let answers = [...this.currentQuestion.incorrect_answers]
            answers.push(this.currentQuestion.correct_answer)
            return _.shuffle(answers);
        }
    },
    methods : {
        selectedAnswer(index){
            this.selectedIndex = index;
            if(this.selectedIndex == this.answers.indexOf(this.currentQuestion.correct_answer)){
                this.isCorrect = true
            }

            var me = document.getElementsByClassName('listBtn');
            var icon = document.createElement('i');
            icon.style.color = 'whitesmoke'
            icon.style.float = 'right'
            icon.style.paddingTop = '5px'
            for(var i=0; i<me.length; i++){
                if(this.selectedIndex !== null){
                    me[this.selectedIndex].appendChild(icon);
                    icon.setAttribute('id','rm');
                    me[i].disabled = true;
                    if(this.selectedIndex === this.answers.indexOf(this.currentQuestion.correct_answer)){
                        me[this.selectedIndex].style.backgroundColor = 'lightgreen';
                        icon.className = 'fa fa-check';
                    }else if(this.selectedIndex !== this.answers.indexOf(this.currentQuestion.correct_answer)){
                        me[this.selectedIndex].style.backgroundColor = '#dd2b2b';
                        icon.className = 'fa fa-times';
                    }
                }
            }
        },
        getCorrectIndex(){
            this.correctIndex = this.answers.indexOf(this.currentQuestion.correct_answer)
        },
        increment(isCorrect){
            if(isCorrect){
                this.numCorrect++
            }
            this.isCorrect = false;
            this.numTotal++;
        },
        // timer(){
        //     // const time_left = document.getElementById('time-left');
        //    setInterval(()=>{
        //        if(this.timeVal <= 0){
        //            clearInterval(this.timeVal = 0);
        //        }
        //        this.timeVal--;
        //    },1000)
        // },
        percentager(){
            this.percent = (this.numCorrect/this.numTotal)*100
            if(this.percent <= 100 && this.percent >= 80){
                this.performance = 'EXCELLENT!!!';
            }
            else if(this.percent >= 65){
                this.performance = 'VERY GOOD!!';
            }
            else if(this.percent >= 50){
                this.performance = 'GOOD!';
            }
            else if(this.percent >= 40){
                this.performance = 'BETTER LUCK NEXT TIME!';
            }
            else if(this.percent < 40){
                this.performance = 'POOR!';
            }
            else{
                this.performance = 'POOR!';
            }
        },
        submitAnswer(){
            var me = document.getElementsByClassName('listBtn');
            // var del_i = document.getElementsByClassName('fa');
            // var del_i = document.getElementsByTagName('i');
            var rm = document.getElementById('rm');
            var parent = rm.parentNode;
            parent.removeChild(rm);
            for(var i=0; i<me.length; i++){
                me[i].disabled = false;
                me[i].style.backgroundColor = '#f8f9fa';
            }   
            // del_i[this.selectedIndex].remove()
            this.answered = true
            this.increment(this.isCorrect);
            this.next();
            this.percentager();
        },
        reloadPage(){
            window.location.reload();
        }
    }
}

</script>


<style scoped>
    button{
       border-width: 0px;
       border-style: solid;
       border-color: transparent;
    }
    button:focus{
       border-width: 0px;
       border-style: solid;
       border-color: transparent;
       outline: 0;
    }
    .list-group{
        width: 80%;
        margin: auto;
        padding: 0px;
    }
    .list-group-item button.btn:hover{
        background-color: rgb(219, 242, 253);
        cursor : pointer;
    }
    .list-group-item button.btn{
        text-align: center;
        display: block;
        margin: 0px;
        border-radius: 0px;
        width: 100%;
        padding-right: 40px;
    }
    .btn{
        margin: 0px 5px;
    }

    .selected{
        background-color: lightblue;
    }
    .correct{
        background-color: lightgreen;
        color: green;
        /* display: inline;
        visibility: visible; */
    }
    .incorrect{
        background-color: red;
        color: red;
        /* display: inline;
        visibility: visible; */
    }
    div#Qnum{
        color: #343a40;
        font-size: 30px;
        font-weight: bold;
    }
    i{
        display : none;
        visibility: hidden;
        float: right;
    }
   .card{
        border-radius: 15px;
    }
    .card,div.list-group,div.list-group-item,.card-body{
        background-color: rgba(255,255,255,0.7);
     }
    hr{
        border: 1px solid #343a40;
        width: 80%;
    }
    button#congrats{
        width: 40%;
        font-size: 15px;
        background-color: #343a40;
        border: 0px;
    }
    button#congrats:hover{
       background-color: #090808;
    }
    .jumbotron{
        background-color: rgba(255, 255, 255, 0.5);
        padding: 0px;
        margin: 0px;
    }
    div#endQuiz{
        margin: 0px;
        background-color: rgba(22, 125, 243, .5);
        text-align: center;
        font-weight: bold;
        font-size: 30px;
    }
    p.info{
        font-size: 18px;
        margin: 3px;    
    }
    div#score{
        background-color: black;
        color:whitesmoke;
        font-size:25px;
        border-radius: 50%;
        width: 150px;
        padding: 12px;
        margin: auto;
        text-align: center;
    }
    button#reload{
        width: 40%;
        margin: auto;
        font-size: 20px;
    }
    div#results{
        margin-top: 20px;
        width: 40%; 
        background-color: rgba(22, 125, 243, .5); 
        margin: auto;
        margin-bottom: 5px;
        border-left: 10px solid rgb(255, 4, 130);
        border-right: 10px solid rgb(255, 4, 130);
    }
</style>