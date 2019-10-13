<template>
  <q-page padding>
    <button class="top"
    @click="counter++">
    {{counter}}</button>

    <input v-model="message"
    @keyup.enter="alertMessage"
    @keyup.esc="clearMessage"
    v-autofocus/>

    <button @click="clearMessage">Clear</button>

    <h5 v-if="message.length"
    class="border-grey" >{{ message }}</h5>

    <h6 v-else>{{noMessage}}</h6>

    <hr>

    <p>UpperCase Message: {{messageUpperCase}}</p>

    <br>

    <p>LowerCase Message: {{message | messageLowerCase}}</p>
  </q-page>
</template>

<script>
export default {
  data(){
    return{
      message: 'I Love You Marinard Florendo',
      noMessage: 'No Message Entered',
      counter: 0
    } 
  },
  computed: {
    messageUpperCase(){
      console.log('Message was being fired')
      return this.message.toUpperCase()
    },
  },
  filters: {
    messageLowerCase(value){
      return value.toLowerCase()
    }
  },
  methods: {
    clearMessage(){
      this.message = ''
    },
    alertMessage(){
      alert(this.message)
    },
  },
  directives: {
    autofocus: {
      inserted(el){
        el.focus()
      }
    }
  }
}
</script>
<style>
  .border-grey{
    border: 15px solid grey;
  }
  .top{
    position: absolute;
    right: 20px;
  }
</style>
