<template>
  <div class="main">
      <div class="container">
        <div class="image">
            <img src="../assets/lisa.gif" alt="gif" class="gif">
        </div>
        <button @click="showJoke" class="btn">Click!</button>
        <div class="joke">
            <p class="joke__content">{{joke.setup}}</p>
            <p class="joke__content">{{joke.delivery}}</p>
            <p class="joke__content">{{joke.oneliner}}</p>
        </div>
        <div class="voices-selector"
        v-if="voices > 0">
            <select 
            v-for="{index, voice} in voices"
            :key="index"
            name="voice-select">
                <option value="{{voice.voiceURI}}">{{voice.name}}</option>
            </select>
        </div>
      </div>
  </div>
</template>

<script>
export default {
    data() {
        return {
            joke: {
                setup: '',
                delivery: '',
                oneliner: '',
            },
            voices: []
        }
    },
    methods: {
        async asyncData() {
            const { setup, delivery, joke } = await fetch('https://v2.jokeapi.dev/joke/Any?lang=en').then( r => r.json() )
            if( setup && delivery ){
            this.joke.setup = setup
            this.joke.delivery = delivery
            this.joke.oneliner = ''
            } else {
                this.joke.oneliner = joke
                this.joke.setup = ''
                this.joke.delivery = ''
            }
            this.readJoke()
        },
        loadVoices( ) {
            new Promise(
            (res) => {
                const synth = window.speechSynthesis.getVoices()
                const id = setInterval(() => {
                if( synth.length !== 0){
                    res(synth)
                    clearInterval(id)
                }
            }, 10)
            synth.forEach(voice => this.voices.push(voice))
            })
            //this.voices.push(voices)
            console.log(this.voices);
        },
        showJoke() {
            this.asyncData()
        },
        readJoke() {
            speechSynthesis.cancel()
            const speech = new SpeechSynthesisUtterance()
            speech.text = `${this.joke.oneliner}${this.joke.setup}${this.joke.delivery}`
            window.speechSynthesis.speak(speech)

        }
    },
    mounted() {
        this.loadVoices()
    }

    
}
</script>

<style scoped>

    .container {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
    }
    .gif {
        max-width: 25vw;
    }

    .btn {
        padding: 10px 25px;
        margin-top: 15px;
        border: none;
        outline: none;
        cursor: pointer;
        color: #fff;
        background-color: #E545FB;
        border-radius: 20px;
        font-size: 26px;
        letter-spacing: 1.5px;
        transition: all .2s ease;
    }

    .btn:hover {
        opacity: .5;
        transform: scale(1.1);
    }
</style>