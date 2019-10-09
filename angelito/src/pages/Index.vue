<template>
  <q-page padding>
    <!-- {{name}}
    {{task}} -->
    <div v-show="show">
    <q-input v-model="name" label="Enter Name" />
    <br>
    <q-input v-model="task" label="Enter Task" />
    <br>
    <!-- with condition -->
    <!-- <q-btn @click="edit ? editedTodo() : addTodo()" class = "full-width" :icon="edit ? 'save' : 'add'" :label="edit ? 'Save':'Add'" color = "deep-orange"/> -->
    <q-btn @click="addTodo()" class = "full-width" icon='add' label='Add' color = "deep-orange"/>
    <br>
    <br>
    </div>
    <!--<pre>{{todos}}</pre>-->

    <!-- <ul v-for="(todo, index) in todos" :key="index" >
      <li>Name: {{todo.name}}</li>
      <li>Task: {{todo.task}}</li>
    <q-btn flat round @click="deleteTodo(index)" icon = "delete" label='Delete' color = "deep-orange"/>
    </ul> -->

    <q-list>
      <!-- <q-item v-show="todo.done === false" clickable :class ="todo.done ? 'bg-green-6': 'bg-red-6'" v-for="(todo, index) in todos" :key="index"> -->
      <q-item clickable :class ="todo.done ? 'bg-green-6': 'bg-red-6'" v-for="(todo, index) in todos" :key="todo._id">
        <q-item-section>
          <q-item-label># {{index + 1}}</q-item-label>
          <q-item-label>Name: {{todo.name}}</q-item-label>
          <q-item-label caption lines="2">Task: {{todo.task}}</q-item-label>
          <q-item-label caption lines="2">Done: {{todo.done}}</q-item-label>
        </q-item-section>

        <q-item-section side>
          <q-btn @click="deleteTodo(todo._id)"  flat round icon = "delete" label='Delete' color = "negative"/>
          <q-btn @click="editTodo(todo, todo._id)" flat round icon = "edit" label='Edit' color = "accent"/>
        </q-item-section>
      </q-item>
    </q-list>

     <q-dialog v-model="edit" persistent>
      <q-card style="min-width: 400px">
        <q-card-section>
          <div class="text-h6">Edit Todo</div>
        </q-card-section>

        <q-card-section>
          <q-input dense v-model="editname" label="Enter Name" />
          <q-input dense v-model="edittask" label="Enter Task" />
          <q-toggle label = "Done" v-model="done"/>
        </q-card-section>

        <q-card-actions align="right" class="text-primary">
          <q-btn flat label="Cancel" icon='close' v-close-popup color = "negative"/>
          <q-btn @click="editedTodo()" icon='save' flat label="Save" v-close-popup color = "secondary" />
        </q-card-actions>      
      </q-card>
    </q-dialog>

      <q-page-sticky position="bottom-right" :offset="[18, 18]">
        <q-btn @click="show = ! show" round color="deep-orange" :icon="show ? 'sentiment_satisfied' : 'sentiment_very_dissatisfied'"/>
      </q-page-sticky>
  </q-page>
</template>

<style>

</style>

<script>
import dbApp from 'src/dbConf.js'
export default {
  name: 'PageIndex',
  data (){
    return{
      name: '',
      task: '',
      editname: '',
      edittask: '',
      done: false,
      todos: [
        // {
        //   name: 'Xander',
        //   task: 'Dance Tommorow',
        //   done: false
        // },
        // {
        //   name: 'Ford',
        //   task: 'Buy Fruits Tommorow',
        //   done: false
        // },
        // {
        //   name: 'Bert',
        //   task: 'Buy Food',
        //   done: true
        // }
      ],
      id: null,
      edit: false,
      show: false
    }
  },
  mounted(){
    // console.log('dsdsds', this.todos)
    dbApp.services.todos.onDataChange(data => { // the function that gives the realtime updates
    // console.log(data)
    this.todos = data // mutate your state. for ReactJS use setState
    })
  },
  methods: {
    addTodo (){

      dbApp.services.todos.create({
        name: this.name, 
        task: this.task, 
        done: false
      })
      //to see the actual result in console
      // .then(result => {
      //   console.log(result)
      // })
      
      //manaully pushing of records
      // this.todos.push({
      //   name: this.name,
      //   task: this.task,
      //   done: false
      // })


      this.name = ''
      this.task = ''
    },
    deleteTodo (id){

      dbApp.services.todos.remove(id)
      //to see the actual result in console
      // .then(result => {
      //   console.log(result)
      // })

      // this.todos.splice(index, 1)
      this.$q.notify({
        color: 'negative',
        message: 'Deleted',
        position: 'top-right',
        timeout: 2500,
        actions: [{icon:'close', color:'white'}]
      })
    },
    editTodo (todo, id){
      this.id = id
      this.editname = todo.name
      this.edittask = todo.task
      this.done = todo.done
      this.edit = true
    },
    editedTodo (){
      // this.todos[this.id].name = this.editname
      // this.todos[this.id].task = this.edittask
      // this.todos[this.id].done = this.done
      dbApp.services.todos.update(
        this.id, 
        {name: this.editname, task: this.edittask, done: this.done})
      this.$q.notify({
        color: 'positive',
        message: 'Saved',
        position: 'top-right',
        timeout: 2500,
        actions: [{icon: 'check', color: 'white'}]
      }),
      this.name = ''
      this.task = ''
    }
  }
}
</script>
