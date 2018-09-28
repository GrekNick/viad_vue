<template>
  <div id="todo-block">
    <section class="panel">	
      <input type="checkbox" id="mark-all" @click="selectAll" :checked="areAllSelected">
      <input v-model="newTask" placeholder="What do you need to do?" autofocus class="text-input">
      <button @click="addTask">Add to List</button>
      <button @click="clearList">Clear List</button>
    </section>
    <section class="list">
      <ul class="list-item">
        <li v-for="task in tasks" v-bind:key="task.id" :class="{done: isChecked(task)}">
          <input type="checkbox" class="checkbox" @click="check" v-model="task.checked">
					<input type="text" 
            class="text-input" 
            v-if="task === editingTask" 
            @keyup.enter="endEditing(task)"
            v-model="task.text">
					<label for="checkbox" v-if="task !== editingTask">{{ task.text }}</label>
          <button @click="editTask(task);endEditing(task)">Edit</button>
          <button class="delete" @click="removeTask(task)">X</button>
        </li>
      </ul>
    </section>
  </div>
</template>

<script>

export default ({
  name: "todo-block",
  
  data () {
    return {
    newTask: "",
    visible: false,
    tasks: [
      {
        text: "This is an example task.",
        checked: false
      }
    ],

    editingTask: {}
    }
  },
  
  computed: {
    areAllSelected: function () {
      return this.tasks.every(function(task) {
        return task.checked;
      }) &&  this.tasks.length > 0;
    },
  },

  methods: {

    addTask: function () {
      var task = this.newTask.trim();
      if (task) {
        this.tasks.push({text: task, checked: false});
        this.newTask = "";
      }
    },

    removeTask: function (task) {
      var index = this.tasks.indexOf(task);
      this.tasks.splice(index, 1);
    },

    addTask: function () {
      var task = this.newTask.trim();
      if (task) {
        this.tasks.push({text: task, checked: false});
        this.newTask = "";
      }
    },

    removeTask: function (task) {
      var index = this.tasks.indexOf(task);
      this.tasks.splice(index, 1);
    },
    editTask: function (task) {
      this.editingTask = task;
    },

    endEditing: function (task) {
      this.editingTask = {};
      if (task.text.trim() === ""){
        this.removeTask(task);
      }
      
    },

    clearList: function () {
      this.tasks = [

      ];
    },

    selectAll: function () {
      var targetValue = this.areAllSelected ? false : true;
      for (var i = 0; i < this.tasks.length; i++) {
        this.tasks[i].checked = targetValue;
      }
    },

    check: function (task) {
      task.checked = true;
    },

    isChecked: function (task) {
      return task.checked;
    }

  }
});
</script>

<style scoped>
ul, li {
	margin: 0;
	padding: 0;
	border: 0;
}


body {
	line-height: 1;
	font-family: "Lato", sans-serif;
	background-color: #EFF1F2;
}

.container {
	width: 70%;
	margin: 1em auto 3em;
	border: 1px solid #efefef;
}

.panel, li {
	display: flex;
	align-items: center;
	justify-content: space-between;
	list-style-type: none;
  position: relative;
	padding: 10px;
	border-bottom: 1px solid #efefef;
	background-color: #E7E8EB;
}

.text-input {
	border: 1px solid #dedede;
	padding-left: 10px;
	width: 70%;
	height: 35px;
	color: #555;
}


button {
	color: #555;
	background-color: #FFFFFF;
	border: 1px solid #bbb;
	outline: 0;
	width: 100px;
	height: 38px;
	cursor: pointer;
	font-size: 14px;
}

.edit_inp{
  height: 30px;
  padding-left: 10px;
  position: absolute;
  left: 140px;
  top: 10px;
  width: 250px;
  z-index: 20;
}

.list li {
	background-color: #3465A4;
}

.list li button {
	background-color: transparent;
	/* border: 1px solid #3465A4; */
	color: #ddd;
	font-size: 20px;
	font-weight: bold;
}


.list label {
	padding-right: 10px;
	display: inline-block;
	width: 70%;
	font-size: 18px;
	line-height: 24px;
	color: #fcfcfc;
	z-index: 2;
	overflow: hidden;
}

.list li.done label {
	color: #d9d9d9;
	text-decoration: line-through;
}
.list li.done{
  background-color: lightcoral;
}


.credit {
	margin: 1em auto 5em;
	text-align: center;
	font-size: 13px;
	line-height: 0.6;
	color: #999;
}

.credit a {
	text-decoration: none;
	color: #999;
}

.credit a:hover {
	text-decoration: underline;
}

.hidden {
	display: none;
}
</style>
