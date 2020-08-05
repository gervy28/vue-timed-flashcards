<template>
    <div class="card relative sm:py-5 md:py-20 my-5 py-10">
        <button class="absolute add-card" @click="addNewCard()" type="button">Add Card</button>
        <div class="question-containers">
            <div v-if="!editQuestion" class="flex flex-row items-baseline justify-center">
                <h3 class="question">{{ flashcards[currentCard].question }}</h3>
                <button class="custom-button text-sm"
                    type="button"
                    v-if="!editQuestion"
                    @click="setupEditQuestion()">
                    Edit Question
                </button>
            </div>

           <div class="m-2" v-if="editQuestion">    
                <input type="text" 
                    class="question question-change p-1"
                    v-model="enteredQuestion"
                >
                <button 
                    class="custom-button"
                    type="button"
                    @click="cancelQuestionEdit()">
                    Cancel
                </button>
                <button 
                    class="custom-button"
                    type="button"
                    @click="replaceQuestion()"
                    >
                    Save
                </button>
           </div>

        </div>
        <!-- show answer section -->
        <div v-if="showAnswer" class="flex flex-col w-full md:flex-row justify-center space-x-3 lg:w-9/12">
            <div class="md:w-1/2 flex flex-col text-left">
                <h4>Correct Answer</h4>
                <p class="p-2 md:h-40 bg-blue-100">
                    {{ flashcards[currentCard].answer }} 
                </p>
            </div>
            <div class="md:w-1/2 flex flex-col text-left">
                <h4>Your Answer:</h4>
                <textarea 
                    class="border-2 h-40 p-2"
                    v-model="enteredAnswer">
                </textarea>
            </div>
        </div>
        <!-- show entered area -->
        <textarea class="w-3/4 md:h-48" v-if="!showAnswer" v-model="enteredAnswer">
        </textarea>
        <!-- Action will be compare or @click show the other answer, might want to set these in rows next to one another -->
        <div class="flex flex-row justify-start space-x-1 my-4">
            <button v-if="!showAnswer" @click="showAnswer = !showAnswer" class="custom-button" type="button">Compare Answer</button>
            <button v-if="showAnswer" class="custom-button bg-green-200" @click="showAnswer = !showAnswer" type="button">Hide Answer</button>
            <button v-if="showAnswer" class="custom-button bg-orange-500" @click="replaceAnswer()" type="button">Replace Answer</button>
        </div>
        <div class="flex flex-row">
            <button v-if="currentCard > 0" class="custom-button" @click="changeCard('previous')" type="button">Previous</button>
            <button v-if="currentCard < totalCards-1" class="custom-button" @click="changeCard('next')" type="button">Next</button>
        </div>
        <span>{{ currentCard + 1 }}/{{ totalCards }}</span>
    </div>
</template>

<script>
import flashcards from "../data/flashcardContent.json";

export default {
    name: 'QuestionCard',
    data() {
        return{
            currentCard: 0,
            editQuestion: false,
            showAnswer: false,
            enteredAnswer: null,
            enteredQuestion: null,
            flashcards : flashcards.flashcards,
            // flashcards: [
            //     {   
            //         'question': "What's the word?",
            //         'answer': "This is the word"
            //     },
            //     {   
            //         'question': "What is functional programing?",
            //         'answer': "Functional programming is a strict type of programming that ensures data.."
            //     },
            //     {
            //         'question': "How do you describe a JavaScript Prototype?",
            //         'answer': "A JavaScript prototype is really just a method or something appended onto the object, that is not defined directly in the object"  
            //     }
            // ]
        }
    },
    computed:{
        totalCards(){
            return this.flashcards.length;
        }
    },
    methods: {
        setupEditQuestion(){
            this.editQuestion = true;
            this.enteredQuestion = this.flashcards[this.currentCard].question; 
        },
        replaceAnswer(){
            if (this.enteredAnswer.length > 0){
                let verify = confirm("Replacing this answer cannot be undone. Click cancel to cancel or okay to replace.")
                if (verify === true){
                    this.flashcards[this.currentCard].answer = this.enteredAnswer;
                } else {
                    alert("Overide Cancelled!")
                }
            } else {
                alert("Answer Cannot Be Blank")
            }
        },
        replaceQuestion(){
            if (this.enteredQuestion.length > 0){
                let verify = confirm("Replacing this question cannot be undone. Click cancel to cancel or okay to replace.")
                if (verify === true){
                    this.flashcards[this.currentCard].question = this.enteredQuestion;
                    this.editQuestion = false;
                    this.enteredQuestion = null;
                } else {
                    alert("Overide Cancelled!")
                }
            } else {
                alert("Answer Cannot Be Blank")
            }
        },
        cancelQuestionEdit(){
            this.editQuestion = false;
            this.enteredQuestion = null;
        },
        changeCard(direction){
            if(direction === "next"){
                this.currentCard++;
            } else {
                this.currentCard--;
            }
            this.showAnswer = false;
            this.enteredAnswer = null;
            this.enteredQuestion = null;
        },
        addNewCard(){
            this.flashcards.push(
                    { 
                        'question': "",
                        'answer': ""
                    }
                );
            // here is one of my first major vue issues, I don't call any changes in the DOM, but I do call changes elsewhere
            this.currentCard = this.flashcards.length - 1;
            this.editQuestion = true;
        }
    }
}
</script>
<style scoped>
.card{
  border-radius: 3.5rem;
  @apply flex;
  @apply flex-col;
  @apply justify-center;
  @apply shadow-lg;
  @apply justify-between;
  @apply font-serif;
  @apply items-center;
}
.question{
 @apply text-3xl;
}
.custom-button{
  @apply border-black;
  @apply rounded-lg;
  @apply px-2;
  @apply py-1;
}

input, textarea{
    @apply border-2;
    @apply p-2;
}

.add-card{
    right: 30px;
    top: 20px;
}

</style>

