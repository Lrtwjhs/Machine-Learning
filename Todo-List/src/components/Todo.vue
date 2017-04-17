<template>
<!-- Main Div Holding our Application Data -->
     <div class="container" id="todo">
     <!-- Panel for holding our input -->
       <section class="panel">
         <input type="checkbox" id="mark-all" v-bind:checked="areAllSelected" v-on:click="selectAll">
         <input type="text" placeholder="what do you need to do?" autofocus class="text-input" v-model="newTask" v-on:keyup.enter="addTask">
         <button v-on:click="clearList">Clear List</button>
        </section>
         <!-- Unorderd list for holding our todo items -->
         <ul class="list">
         <li v-for="task in taskList" v-bind:class="{done: task.checked}">
            <input type="checkbox" class="checkbox" v-model="task.checked">
            <label for="checkbox">{{ task.text }}</label>
            <button class="delete" v-on:click="removeTask(task)">X</button>
         </li>
         </ul>
      <!-- This stringifies the contents of the data object and displays it on the page -->
      <pre>{{ $data }}</pre>
     </div>
</template>

<script>
export default {
  // Bind this Vue instance to our container div with an ID of todo
  name: 'todo',

  // This is where the data for our application will be held
  data () {
    return {
      newTask: '',
      taskList: []
    }
  },

  computed: {
    areAllSelected: function () {
      // Check if every checked property returns true and if there is at least one todo item
      return this.taskList.every(function (task) {
        return task.checked
      }) && this.taskList.length > 0
    }
  },

  methods: {
    addTask: function () {
      // trim() is used to remove whitespace from both ends of a string
      var task = this.newTask.trim()
      // if task is not an empty string
      if (task) {
        // Push an object containing the task to the taskList array
        this.taskList.push({
          text: task,
          checked: false
        })
        // Reset newTask to an empty string so the input field is cleared
        this.newTask = ''
      }
    },

    removeTask: function (task) {
      var index = this.taskList.indexOf(task)
      this.taskList.splice(index, 1)
    },

    clearList: function () {
       // Setting taskList to an empty array clears the whole list
      this.taskList = []
    },

    selectAll: function (task) {
      // targetValue is set to the opposite of areAllSelected
      var targetValue = !this.areAllSelected
      // we use a for loop to set the checked state of all items to the target value
      for (var i = 0; i < this.taskList.length; i++) {
        this.taskList[i].checked = targetValue
      }
    }
  }
}
</script>

<style>
   /* Reset the default browser styles on list items */
    ul,
    li{
      margin: 0;
      padding: 0;
      border: 0;
    }

    /* Global Styles */
    body{
      line-height: 1;
      font-family: "Lato", sans-serif;
      background-color: #EFF1F2;
    }
    .container{
      width: 70%;
      margin: 1em auto 3em;
      border: 1px solid #efefef;
    }
    .panel,
    li{
      /* Use flexbox */
      display: flex;
      /* Center everything inside .panel and vertically */
      align-items: center;
      /* Distribute space evenly between the contents */
      justify-content: space-between;
      list-style-type: none;
      padding: 10px;
      border-bottom: 1px solid #efefef;
      background-color: #E7E8EB;
    }

    .text-input{
      border: 1px solid #dedede;
      padding-left: 10px;
      width: 70%;
      height: 35px;
      color: #555;
    }

    button{
      color: #555;
      background-color: #FFFFFF;
      border: 1px solid #bbb;
      outline: 0;
      width: 100px;
      height: 38px;
      cursor: pointer;
      font-size: 12px;
    }

    /* Task area */
    .list li{
      background-color: #3465A4;
    }
    .list li .delete{
      background-color: transparent;
      border: 1px solid #3465A4;
      color: #ddd;
      /* Hide the delete button by default */
      visibility: hidden;
      font-size: 20px;
      font-weight: bold;
    }
    .list li:hover > .delete{
      /* Show the delete button when hover each list item */
      visibility: visible;
    }
    .list label{
      padding-right: 10px;
      display: inline-block;
      width: 70%;
      font-size: 18px;
      line-height: 24px;
      color: #fcfcfc;
      z-index: 2;
      overflow: hidden;
    }
    .list li.done label{
      color: #d9d9d9;
      text-decoration: line-through;
    }

    /* Media Queries */
    @media screen and (max-width: 768px){
      .container{
        width: 90%;
        max-width: 90%;
      }
      button{
        width: 80px;
      }
    }
</style>
