<template>
    <q-page padding>
        <input v-model.number="num"/>
        <h1>Hello {{greetings}}</h1>
        <h1>{{num - num1}}</h1>
        <h2>Addition: {{addition}}</h2>
        <h2>Subtraction: {{subtraction}}</h2>
    </q-page>
</template>

<script>
export default {
    data () {
        return {
            greetings: '',
            num: 0,
            num1: 3
        }
    },
    computed: {
        addition () {
            return this.num + this.num1
        },
        subtraction () {
            return this.num - this.num1
        }
        
    }
}
</script>

<style>

</style>
