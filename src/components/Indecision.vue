<template>
    <img v-if="img" :src="img" alt="bg">
    <!-- <div class="bg-dark"></div> -->

    <div class="indecision-container">

        <input
            v-model="question"
            type="text"
            placeholder="Hazme una pregunta">
        <p>Recuerda terminar con un signo de interrogación (?)</p>

        <div v-if="isValidQuestion">
            <h2>{{ question }}</h2>
            <h1>{{ answer }}</h1>
            <!-- Si!: YES -->
            <!-- No!: No -->
            <br><br>
            <p>Chiste de chucnorris</p>
            <h3>    {{ id }}        </h3>
            <h1> {{ value }}  </h1>
        </div>

    </div>


</template>

<script>
  
  const URI= 'https://api.chucknorris.io/jokes/random'

export default {
  
    data() {
        return {
            question: null,
            answer: null,
            img: null,
            isValidQuestion: false,
            url:null,
            urlchuck:null,
            value:null,
            id:null
        }
    },
    methods: {

        async getAnswer() {

            this.urlchuck= URI

            this.url='https://yesno.wtf/api'
            
            this.answer = 'Pensando...'

            
            const { answer, image } = await fetch(this.url).then( r => r.json() )
            
            // const { answer, image } = await fetch('https://yesno.wtf/api').then( r => r.json() )

            this.answer = answer === 'yes' ? 'Si!' : 'No!'
            
            this.img = image

            // chucnorris cuentos
            const { id, value } = await fetch(URI).then( rrr => rrr.json() ) 
            this.value=value
            this.id=id


        }
    },
    watch: {
        question( value ){

            this.isValidQuestion = false
            
            this.img = ""
            // console.log('antes de if');
            if( !value.includes('?') ) return 
                // console.log('despues de teclear?');
            this.isValidQuestion = true

            // TODO: Realizar petición http
            this.getAnswer()
        }
    }

}
</script>

<style>

    img, .bg-dark {
        height: 100vh;
        left: 0px;
        max-height: 100%;
        max-width: 100%;
        position: fixed;
        top: 0px;
        width: 100vw;
    }

    .bg-dark {
        background-color: rgba(226, 97, 97, 0.4);
    }

    .indecision-container {
        position: relative;
        z-index: 99;
    }
    
    input {
        width: 250px;
        padding: 10px 15px;
        border-radius: 5px;
        border: none;
    }
    input:focus {
        outline: none;
    }

    p {
        color: white;
        font-size: 20px;
        margin-top: 0px;
    }

    h1, h2 ,h3 {
        color: white;
    }
    
    h2 {
        margin-top: 150px;
    }

</style>