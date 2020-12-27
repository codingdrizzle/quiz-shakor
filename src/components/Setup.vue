<template id='me'>
    <div class="container mt-5">
        <div class="row">
            <div class="col-sm-12 col-md-12 col-lg-12 col-xs-12">
                <div class="p-1 jumbotron">
                    <p class="lead m-0 p-0 text-center">Setup your QUIZ !</p>
                </div>
            </div>
        </div>
        <form action="#" class="d-flex justify-content-center mt-3">
            <div class="form-group">
                <!-- User choose number of question to answer in the quiz -->
                <p class="broz">Number of Questions</p>
                <select class="form-control mb-1"  name="amount" id="numQuestions" @change="numQuestions" 
                >
                    <option value="10">10</option>
                    <option value="15">15</option>
                    <option value="25">25</option>
                    <option value="30">30</option>
                </select>
                <!-- User selects quiz category/ quiz topic -->
                <p class="broz">Category</p>
                <select class="form-control mb-1" name="category" id="Category" @change="Category">
                    <option value="">Any Category</option>
                    <option value="9">General Knowledge</option>
                    <option value="10">Entertainment: Books</option>
                    <option value="11">Entertainment: Film</option>
                    <option value="12">Entertainment: Music</option>
                    <option value="13">Entertainment: Musicals and Theatres</option>
                    <option value="14">Entertainment: Television</option>
                    <option value="15">Entertainment: Video Games</option>
                    <option value="16">Entertainment: Board Games</option>
                    <option value="17">Science and Nature</option>
                    <option value="18">Science: Computers</option>
                    <option value="19">Science: Mathematics</option>
                    <option value="20">Mythology</option>
                    <option value="21">Sports</option>
                    <option value="22">Geography</option>
                    <option value="23">History</option>
                    <option value="24">Politics</option>
                    <option value="25">Art</option>
                    <option value="26">Celebrities</option>
                    <option value="27">Animals</option>
                    <option value="28">Vehicles</option>
                    <option value="29">Entertainment: Comics</option>
                    <option value="30">Science: Gadgets</option>
                    <option value="31">Entertainment: Japanese Anime and Manga</option>
                    <option value="32">Entertainment: Cartoon and Animations</option>
                </select>
                <!-- User selects difficulty level -->
                <p class="broz">Difficulty Level</p>
                <select class="form-control mb-1 quizSetup" name="difficulty" id="Difficulty" @change="Difficulty">
                    <option value="medium">Any Difficulty</option>
                    <option value="easy">Easy</option>
                    <option value="medium">Medium</option>
                    <option value="hard">Hard</option>
                </select>
                <!-- User chooses quiz type (True/False or Multiple Choice) -->
                <p class="broz">Quiz Type</p>
                <select class="form-control mb-1 quizSetup" name="type" id="Type" @change="Type">
                    <option value="multiple">Any Type</option>
                    <option value="multiple">Multiple Choice</option>
                    <option value="boolean">True / False</option>
                </select>
                <button type="Submit" 
                v-on:click = "caller"
                @click="$emit('generator',[num_questions, category, difficulty, type])" 
                v-on:click.prevent.stop class="form-control btn btn-primary mt-4"
                > Take Quiz</button>
            </div>
        </form>
    </div>
</template>

<script>
    // import App from '../App.vue'
    export default { 
        name : 'Setup', 
        data(){
            return {
                num_questions : 10,
                category : null,
                difficulty : null,
                type : null
            }
        }, 
        methods : {
            numQuestions(){
            var numQuestions = document.getElementById("numQuestions");
                this.num_questions = numQuestions.value;
            },
            Category(){
                var Category = document.getElementById("Category");
                if(Category.value ==  ""){
                    this.category = Math.floor(Math.random() *(32 - 9 + 1))+9;
                }else{
                    this.category = Category.value;
                }
            },
            Difficulty(){
                var Difficulty = document.getElementById("Difficulty");
                this.difficulty = Difficulty.value;
            },
            Type(){
                var Type = document.getElementById("Type");
                this.type = Type.value;
            },
            caller(){
                this.numQuestions(),
                this.Category(),
                this.Difficulty(),
                this.Type()
                console.log("Caller Function called")
            }
        }
    }

</script>

<style scoped>
    div.jumbotron{
        width: 90%;
        margin: auto;
    }
    .form-group{
        width: 90%;
    }
    .jumbotron{
        background-color: #91c5fc;
        border: 2px solid #007bff;
    }
    p.lead{
        font-weight: bold;
        color: #007bff;
        font-size: 22px;
    }
    select{
        color: #007bff;
    }
    select:hover{
        cursor: pointer;
    }
    select:focus {
        outline: none;
    }
    .broz{
        text-align: center;
        margin-bottom: 10px;
        color: whitesmoke;
        font-weight: bold;
        letter-spacing: 1px;
    }
    button.btn{
        font-size: 20px;
    }
</style>